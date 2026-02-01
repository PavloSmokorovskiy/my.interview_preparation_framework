# System Design Interview Template

> **Время:** Используй этот шаблон во время интервью (15-20 минут на вопрос)

---

## Этап 1: Requirements (2-3 минуты)

### Functional Requirements
- [ ] **Core features:** Что система должна делать?
- [ ] **User actions:** Какие операции поддерживаем?
- [ ] **Edge cases:** Особые сценарии?

### Non-functional Requirements
- [ ] **Scale:**
  - DAU/MAU: ___
  - QPS (read): ___
  - QPS (write): ___

- [ ] **Performance:**
  - Latency (p99): ___ ms
  - Throughput: ___ req/s

- [ ] **Availability:**
  - SLA: ___ % (99.9% = 8.76 hrs/year downtime)

- [ ] **Consistency:**
  - Strong / Eventual?

### Constraints
- [ ] Storage: ___ TB/PB
- [ ] Budget: Startup / Enterprise
- [ ] Geo: Single region / Global

---

## Этап 2: High-Level Design (3-5 минут)

### Components (нарисуй)

```
┌─────────┐     ┌──────────────┐     ┌─────────────┐
│ Clients │────▶│ Load Balancer│────▶│ App Servers │
└─────────┘     └──────────────┘     └──────┬──────┘
                                            │
                    ┌───────────────────────┼───────────────────────┐
                    │                       │                       │
                    ▼                       ▼                       ▼
              ┌──────────┐           ┌──────────┐           ┌──────────┐
              │  Cache   │           │ Database │           │  Queue   │
              └──────────┘           └──────────┘           └──────────┘
```

### API Design

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/v1/___` | GET | |
| `/api/v1/___` | POST | |
| `/api/v1/___` | PUT | |
| `/api/v1/___` | DELETE | |

### Data Flow
1. Client →
2. →
3. →
4. Response ←

---

## Этап 3: Deep Dive (5-8 минут)

### Data Model

**Table/Collection: ___**
| Field | Type | Notes |
|-------|------|-------|
| id | UUID/BigInt | Primary key |
| | | |
| | | |
| created_at | Timestamp | |

**Indexes:**
- Primary: `id`
- Secondary: `___`

### Key Decisions

| Decision | Options | Chosen | Why |
|----------|---------|--------|-----|
| Database | SQL / NoSQL | | |
| Cache | Redis / Memcached | | |
| Queue | Kafka / SQS | | |

### Component Details

**Component 1: ___**
- Responsibility:
- Scale strategy:
- Failure handling:

**Component 2: ___**
- Responsibility:
- Scale strategy:
- Failure handling:

---

## Этап 4: Scaling & Reliability (3-5 минут)

### Bottlenecks
1. ___: Решение: ___
2. ___: Решение: ___

### Scaling Strategy

| Component | Strategy | Details |
|-----------|----------|---------|
| App Servers | Horizontal | Auto-scaling group |
| Database | | Read replicas / Sharding |
| Cache | | Cluster mode |

### Failure Scenarios

| Scenario | Impact | Mitigation |
|----------|--------|------------|
| DB failure | | Failover to replica |
| Cache failure | | Fallback to DB |
| Server crash | | Health checks, restart |

### Monitoring
- **Metrics:** QPS, latency, error rate, CPU/Memory
- **Alerts:** Error rate > X%, Latency > Y ms
- **Logging:** Structured logs, trace IDs

---

## Quick Reference: Typical Numbers

### Latency
- In-memory: 1 μs
- SSD: 100 μs
- Network (DC): 0.5 ms
- Network (cross-region): 100 ms

### Throughput
- Single server: 1K-10K QPS
- Redis: 100K+ QPS
- Kafka: 1M+ messages/sec

### Storage
- 1 char = 1 byte (ASCII), 2-4 bytes (UTF-8)
- 1 KB = 1,000 chars
- 1 MB = 1,000 KB
- 1 GB = 1,000 MB
- 1 TB = 1,000 GB

### Time conversions
- 1 day = 86,400 sec ≈ 100K sec
- 1 week = 604,800 sec ≈ 600K sec
- 1 month = 2.6M sec
- 1 year = 31.5M sec

---

## Phrases for Interview

### Clarifying
- "Before diving in, let me clarify the requirements..."
- "What's the expected scale? DAU, QPS?"
- "Should we optimize for read or write?"

### Designing
- "Let me start with a high-level architecture..."
- "The main components are..."
- "Data flows from... to..."

### Trade-offs
- "There's a trade-off between X and Y..."
- "We could use A or B. A is better for... but B..."
- "Given the requirements, I'd choose... because..."

### Scaling
- "To handle more traffic, we can..."
- "The bottleneck here is... we can address it by..."
- "For high availability, we need..."
