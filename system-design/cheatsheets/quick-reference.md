# System Design Quick Reference

> **Время изучения:** ~5 минут

Краткая справка по ключевым концепциям System Design.

---

## CAP Theorem

| | Consistency | Availability |
|-|-------------|--------------|
| **CP** | ✅ | ❌ (при partition) |
| **AP** | ❌ (eventual) | ✅ |

**CP примеры:** Banks, inventory systems
**AP примеры:** Social media, DNS

---

## Когда что использовать

### Database Selection

| Нужно | Используй |
|-------|-----------|
| ACID, complex queries | PostgreSQL, MySQL |
| Flexible schema, documents | MongoDB |
| High write throughput | Cassandra |
| Key-value, sessions | Redis |
| Relationships, graphs | Neo4j |
| Full-text search | Elasticsearch |

### Caching Strategy

| Сценарий | Стратегия |
|----------|-----------|
| Read-heavy, data rarely changes | Cache-aside |
| Data must be consistent | Write-through |
| Write-heavy, eventual consistency OK | Write-behind |

### Message Queue Selection

| Нужно | Используй |
|-------|-----------|
| High throughput, log aggregation | Kafka |
| Complex routing, RPC | RabbitMQ |
| Simple, managed | AWS SQS |
| Real-time streaming | Kafka Streams |

---

## Scaling Patterns

### Database

```
1. Vertical scaling (bigger machine)
          ↓
2. Read replicas (separate read/write)
          ↓
3. Caching (reduce DB load)
          ↓
4. Sharding (split data)
```

### Application

```
1. Single server
          ↓
2. Load balancer + multiple servers
          ↓
3. Stateless design (any server can handle)
          ↓
4. Auto-scaling (based on load)
```

---

## Common Calculations

### QPS Estimates

| Daily requests | QPS |
|----------------|-----|
| 100K | ~1 |
| 1M | ~12 |
| 10M | ~120 |
| 100M | ~1.2K |
| 1B | ~12K |

### Storage Estimates

| Item | Size |
|------|------|
| UUID | 16 bytes |
| Timestamp | 8 bytes |
| Int | 4 bytes |
| Short text (100 chars) | 100 bytes |
| URL | 200 bytes |
| Image thumbnail | 10 KB |
| Image full | 200 KB |
| Video minute (compressed) | 50 MB |

### Bandwidth

```
Bandwidth = QPS × Request Size

Example:
- 10K QPS, 1 KB request
- Bandwidth = 10K × 1 KB = 10 MB/s
```

---

## Reliability Numbers

### Availability

| SLA | Downtime/year | Downtime/month |
|-----|---------------|----------------|
| 99% | 3.65 days | 7.3 hours |
| 99.9% | 8.76 hours | 43 min |
| 99.99% | 52.6 min | 4.3 min |
| 99.999% | 5.26 min | 26 sec |

### Replication

| Strategy | Consistency | Availability |
|----------|-------------|--------------|
| Single master | Strong | Lower |
| Multi-master | Eventual | Higher |
| Quorum (W+R>N) | Tunable | Tunable |

---

## Common Components

```
┌─────────────────────────────────────────────────────────┐
│                         CDN                             │
└─────────────────────────────────────────────────────────┘
                            │
┌─────────────────────────────────────────────────────────┐
│                    Load Balancer                        │
└─────────────────────────────────────────────────────────┘
                            │
┌─────────────────────────────────────────────────────────┐
│                    API Gateway                          │
│         (Auth, Rate Limiting, Routing)                  │
└─────────────────────────────────────────────────────────┘
                            │
         ┌──────────────────┼──────────────────┐
         │                  │                  │
    ┌────▼────┐       ┌─────▼────┐       ┌─────▼────┐
    │ Service │       │ Service  │       │ Service  │
    │    A    │       │    B     │       │    C     │
    └────┬────┘       └────┬─────┘       └────┬─────┘
         │                 │                  │
         └────────────┬────┴────┬─────────────┘
                      │         │
                 ┌────▼───┐ ┌───▼────┐
                 │ Cache  │ │ Queue  │
                 └────┬───┘ └───┬────┘
                      │         │
                 ┌────▼─────────▼────┐
                 │     Database      │
                 │  (Primary/Replica)│
                 └───────────────────┘
```

---

## Trade-offs Cheat Sheet

| Trade-off | Option A | Option B |
|-----------|----------|----------|
| Consistency vs Availability | Strong consistency | High availability |
| Latency vs Throughput | Low latency | High throughput |
| Simplicity vs Features | Simple, limited | Complex, full-featured |
| SQL vs NoSQL | ACID, joins | Scale, flexibility |
| Push vs Pull | Real-time, more resources | On-demand, simpler |
| Cache vs DB | Fast, stale data possible | Slow, always fresh |
| Monolith vs Microservices | Simple, coupled | Complex, independent |

---

## Red Flags to Avoid

| ❌ Don't | ✅ Do |
|---------|-------|
| Jump to solution | Clarify requirements first |
| One "right" answer | Discuss trade-offs |
| Ignore scale | Ask about QPS, storage |
| Over-engineer | Start simple, evolve |
| Silent design | Think out loud |
| Forget failures | Discuss failure modes |
