# Common System Design Problems

> **Время изучения:** ~20 минут

Разбор типовых систем, которые часто спрашивают на интервью.

---

## 1. URL Shortener (TinyURL)

### Requirements

**Functional:**
- Shorten long URL to short URL
- Redirect short URL to original
- Custom short URLs (optional)
- Analytics (optional)

**Non-functional:**
- 100M URLs/day created
- 10:1 read:write ratio → 1B redirects/day
- URL expiration (optional)
- High availability

### Calculations

```
Write QPS: 100M / 86400 ≈ 1.2K QPS
Read QPS: 1B / 86400 ≈ 12K QPS
Storage (5 years): 100M × 365 × 5 × 500 bytes ≈ 100 TB
```

### High-Level Design

```
┌────────┐     ┌──────────────┐     ┌─────────────┐
│ Client │────▶│ Load Balancer│────▶│ App Servers │
└────────┘     └──────────────┘     └──────┬──────┘
                                           │
                         ┌─────────────────┴─────────────────┐
                         │                                   │
                    ┌────▼────┐                        ┌─────▼─────┐
                    │  Cache  │                        │  Database │
                    │ (Redis) │                        │ (Key-Val) │
                    └─────────┘                        └───────────┘
```

### Key Design Decisions

**ID Generation:**

| Approach | Pros | Cons |
|----------|------|------|
| Hash (MD5, SHA) | Deterministic | Collisions |
| Counter + Base62 | No collisions | Single point |
| UUID | Distributed | Long |
| Snowflake ID | Distributed, sortable | Complexity |

**Recommendation:** Base62 encoding of auto-increment ID

```
Base62: [0-9a-zA-Z] = 62 characters
6 chars = 62^6 = 56.8 billion URLs
7 chars = 62^7 = 3.5 trillion URLs
```

**Database:**
- Key-value store (Redis, DynamoDB)
- Key: short_url, Value: {long_url, created_at, expiry}

### API Design

```
POST /api/v1/shorten
Body: { "long_url": "https://...", "custom_alias": "my-link" }
Response: { "short_url": "https://tiny.url/abc123" }

GET /{short_url}
Response: 301 Redirect to long_url
```

---

## 2. Rate Limiter

### Requirements

**Functional:**
- Limit requests per user/IP
- Return 429 when limit exceeded
- Different limits for different APIs

**Non-functional:**
- Low latency (< 1ms added)
- Distributed (multiple servers)
- Accurate counting

### Algorithms

**1. Token Bucket** (рекомендуется)

```
┌─────────────────┐
│ Bucket          │
│ Capacity: 10    │    Tokens added: 1/sec
│ Tokens: 7       │◄─────────────────────
│                 │
└────────┬────────┘
         │
         ▼ Take token per request
     [Request]
```

- Tokens added at fixed rate
- Request takes 1 token
- If no tokens → reject

**2. Sliding Window Log**

```
Window: last 60 seconds
Requests: [t1, t2, t3, t4, ...]
Count requests in window
```

- Store timestamp of each request
- Count requests in sliding window
- More accurate, more memory

**3. Sliding Window Counter**

```
Current window (60s): 30 requests
Previous window (60s): 50 requests
Position in current: 25% through

Weighted count = 30 + 50 × 0.75 = 67.5
```

- Hybrid approach
- Less memory than log
- Approximate

### Implementation (Redis)

```
-- Token Bucket in Redis
Key: rate_limit:{user_id}
Value: { tokens: N, last_refill: timestamp }

-- Sliding Window in Redis
Key: rate_limit:{user_id}:{minute}
Value: count
TTL: 60 seconds
```

### Architecture

```
┌────────┐     ┌─────────────┐     ┌─────────────┐
│ Client │────▶│ Rate Limiter│────▶│ App Server  │
└────────┘     │  Middleware │     └─────────────┘
               └──────┬──────┘
                      │
                ┌─────▼─────┐
                │   Redis   │
                │  Cluster  │
                └───────────┘
```

---

## 3. Chat System (WhatsApp/Slack)

### Requirements

**Functional:**
- 1:1 messaging
- Group chat (up to 500 members)
- Online status
- Read receipts
- Push notifications

**Non-functional:**
- 50M DAU
- Low latency (< 500ms delivery)
- Message persistence
- Exactly-once delivery

### High-Level Design

```
┌────────┐     ┌──────────────┐     ┌──────────────────┐
│ Client │◄───▶│  WebSocket   │◄───▶│  Chat Service    │
└────────┘     │   Servers    │     └────────┬─────────┘
               └──────────────┘              │
                                    ┌────────┴────────┐
                                    │                 │
                              ┌─────▼─────┐    ┌──────▼──────┐
                              │  Message  │    │   User      │
                              │  Queue    │    │   Service   │
                              └─────┬─────┘    └─────────────┘
                                    │
                              ┌─────▼─────┐
                              │  Message  │
                              │    DB     │
                              └───────────┘
```

### Key Components

**1. WebSocket Server:**
- Maintains persistent connections
- Routes messages to recipients
- Handles connection/disconnection

**2. Message Queue:**
- Decouples sending and receiving
- Handles offline users
- Ensures delivery

**3. Message Storage:**

| Field | Type |
|-------|------|
| message_id | UUID |
| conversation_id | UUID |
| sender_id | UUID |
| content | Text |
| timestamp | Timestamp |
| status | Enum (sent, delivered, read) |

**Partitioning:** By conversation_id

### Message Flow

**Sending:**
```
1. Client A sends message via WebSocket
2. Server validates, stores in DB
3. If Client B online: deliver via WebSocket
4. If Client B offline: store, push notification
```

**Delivery Guarantees:**
- Message ID for deduplication
- Acknowledgments at each step
- Retry with exponential backoff

---

## 4. News Feed (Facebook/Twitter)

### Requirements

**Functional:**
- Create posts
- Follow users
- View personalized feed
- Like, comment

**Non-functional:**
- 200M DAU
- Feed generation < 500ms
- Fresh content (< 5 min delay acceptable)

### Approaches

**1. Pull Model (Fan-out on Read)**

```
User opens feed:
1. Get list of followed users
2. Fetch recent posts from each
3. Merge and rank
4. Return top N
```

| Pros | Cons |
|------|------|
| Simple | Slow for users with many followings |
| No wasted computation | High read latency |

**2. Push Model (Fan-out on Write)**

```
User creates post:
1. Get list of followers
2. Write to each follower's feed cache
```

| Pros | Cons |
|------|------|
| Fast reads | Slow writes for celebrities |
| Pre-computed | Storage overhead |

**3. Hybrid (Recommended)**

```
- For normal users: Push model
- For celebrities (>1M followers): Pull model at read time
```

### Architecture

```
┌────────┐     ┌──────────────┐
│ Client │────▶│ Feed Service │
└────────┘     └──────┬───────┘
                      │
         ┌────────────┼────────────┐
         │            │            │
    ┌────▼────┐  ┌────▼────┐  ┌────▼────┐
    │  Feed   │  │  Post   │  │  Graph  │
    │  Cache  │  │ Service │  │ Service │
    └────┬────┘  └────┬────┘  └────┬────┘
         │            │            │
         └────────────┼────────────┘
                      │
                ┌─────▼─────┐
                │  Database │
                └───────────┘
```

### Feed Generation

```
1. Get candidate posts (from cache or query)
2. Filter (blocked users, seen posts)
3. Rank (engagement, recency, relevance)
4. Return top N with pagination
```

---

## 5. Distributed Cache (Redis-like)

### Requirements

**Functional:**
- GET, SET, DELETE operations
- TTL support
- Different data types (string, list, hash)

**Non-functional:**
- < 1ms latency
- High availability
- Horizontal scaling
- 1M+ QPS

### Architecture

```
┌────────┐     ┌──────────────┐     ┌─────────────────┐
│ Client │────▶│ Proxy/Router │────▶│ Cache Cluster   │
└────────┘     └──────────────┘     │ ┌─────┐ ┌─────┐ │
                                    │ │Node1│ │Node2│ │
                                    │ └─────┘ └─────┘ │
                                    │ ┌─────┐ ┌─────┐ │
                                    │ │Node3│ │Node4│ │
                                    │ └─────┘ └─────┘ │
                                    └─────────────────┘
```

### Key Concepts

**1. Consistent Hashing:**
- Keys distributed across nodes
- Minimal redistribution when nodes added/removed

```
Hash Ring:
     Node A (0-25%)
        │
   ┌────┴────┐
   │         │
Node D    Node B
(75-100%)  (25-50%)
   │         │
   └────┬────┘
        │
     Node C (50-75%)
```

**2. Replication:**
- Each key replicated to N nodes
- Writes to primary, async to replicas
- Reads from any replica

**3. Eviction Policies:**
- LRU (Least Recently Used)
- LFU (Least Frequently Used)
- TTL-based expiration

---

## 6. Notification System

### Requirements

**Functional:**
- Push notifications (iOS, Android, Web)
- Email, SMS
- User preferences
- Batching/throttling

**Non-functional:**
- 100M notifications/day
- < 1 min delivery time
- At-least-once delivery
- Rate limiting

### Architecture

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Event Source │────▶│ Notification │────▶│   Workers    │
│ (Services)   │     │    Queue     │     │              │
└──────────────┘     └──────────────┘     └──────┬───────┘
                                                  │
                     ┌────────────────────────────┼────────────────────────────┐
                     │                            │                            │
               ┌─────▼─────┐               ┌──────▼─────┐              ┌───────▼──────┐
               │   Push    │               │   Email    │              │     SMS      │
               │ Provider  │               │  Provider  │              │   Provider   │
               │(APNS/FCM) │               │(SendGrid)  │              │  (Twilio)    │
               └───────────┘               └────────────┘              └──────────────┘
```

### Key Components

**1. Notification Service:**
- Receives notification requests
- Validates, enriches
- Routes to appropriate queue

**2. User Preferences:**
- Channels (push, email, SMS)
- Frequency (immediate, digest)
- Quiet hours

**3. Template Engine:**
- Personalization
- Localization
- A/B testing

**4. Delivery Tracking:**
- Sent, delivered, opened
- Retry failed deliveries
- Analytics

---

## Summary: Common Patterns

| System | Key Insight |
|--------|-------------|
| URL Shortener | ID generation, caching |
| Rate Limiter | Token bucket, distributed counting |
| Chat | WebSocket, message queue, presence |
| News Feed | Fan-out strategies, caching |
| Cache | Consistent hashing, replication |
| Notifications | Queues, providers, preferences |

### Universal Components

Почти каждая система включает:
1. **Load Balancer** — распределение нагрузки
2. **Cache** — уменьшение latency
3. **Queue** — async processing
4. **Database** — persistence
5. **Monitoring** — observability
