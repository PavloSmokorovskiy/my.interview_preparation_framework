# Caching

> **Время изучения:** ~10 минут

Стратегии кэширования для улучшения производительности.

---

## Зачем кэширование

- **Снижение latency:** Memory vs Disk/Network
- **Уменьшение нагрузки на DB:** Меньше запросов
- **Повышение throughput:** Больше запросов в секунду
- **Cost reduction:** Меньше серверов БД

---

## Где кэшировать

### Cache Layers

```
┌─────────┐
│ Browser │ ← Browser Cache, LocalStorage
└────┬────┘
     │
┌────▼────┐
│   CDN   │ ← Static content, edge caching
└────┬────┘
     │
┌────▼────┐
│   API   │ ← Response cache
│ Gateway │
└────┬────┘
     │
┌────▼────┐
│   App   │ ← In-memory cache (local)
│ Server  │
└────┬────┘
     │
┌────▼────┐
│  Redis  │ ← Distributed cache
│Memcached│
└────┬────┘
     │
┌────▼────┐
│Database │ ← Query cache, buffer pool
└─────────┘
```

---

## Caching Strategies

### 1. Cache-Aside (Lazy Loading)

```
Read:
┌───────┐    1. Check cache    ┌───────┐
│ App   │ ──────────────────▶ │ Cache │
└───┬───┘    2. Cache miss     └───────┘
    │        3. Read from DB
    │        ┌───────┐
    └───────▶│  DB   │
             └───────┘
    4. Update cache

Write:
┌───────┐    1. Write to DB    ┌───────┐
│ App   │ ──────────────────▶ │  DB   │
└───┬───┘    2. Invalidate     └───────┘
    │        ┌───────┐
    └───────▶│ Cache │
             └───────┘
```

**Плюсы:**
- Простота
- Cache содержит только нужные данные
- Resilient to cache failures

**Минусы:**
- Cache miss penalty
- Возможна stale data
- Холодный старт

### 2. Write-Through

```
Write:
┌───────┐    1. Write         ┌───────┐    2. Write     ┌───────┐
│ App   │ ──────────────────▶│ Cache │ ─────────────▶ │  DB   │
└───────┘                     └───────┘                └───────┘
         3. Ack after both complete
```

**Плюсы:**
- Данные в cache всегда свежие
- Простая consistency модель

**Минусы:**
- Higher write latency
- Cache может содержать ненужные данные

### 3. Write-Behind (Write-Back)

```
Write:
┌───────┐    1. Write         ┌───────┐
│ App   │ ──────────────────▶│ Cache │ ──┐
└───────┘    2. Ack immediately└───────┘  │
                                          │ 3. Async
                                          │    write
                                    ┌─────▼───┐
                                    │   DB    │
                                    └─────────┘
```

**Плюсы:**
- Низкая write latency
- Batch writes возможен

**Минусы:**
- Risk of data loss
- Complexity
- Eventual consistency

### 4. Read-Through

```
Read:
┌───────┐    1. Read          ┌───────┐
│ App   │ ──────────────────▶│ Cache │
└───────┘                     └───┬───┘
                                  │ 2. If miss, cache
                                  │    loads from DB
                            ┌─────▼───┐
                            │   DB    │
                            └─────────┘
```

Cache отвечает за загрузку данных из DB.

---

## Cache Invalidation

### Strategies

| Strategy | Description | When to use |
|----------|-------------|-------------|
| **TTL** | Expire after time | When staleness OK |
| **Event-based** | Invalidate on update | Real-time needs |
| **Write-through** | Update with write | Consistency critical |
| **Manual purge** | Explicit invalidation | Infrequent updates |

### TTL Best Practices

```
Short TTL (seconds-minutes):
- Frequently changing data
- User sessions

Medium TTL (minutes-hours):
- Product catalog
- User profiles

Long TTL (hours-days):
- Static content
- Configuration
```

---

## Eviction Policies

Когда cache заполнен, что удалять?

| Policy | Description | Best for |
|--------|-------------|----------|
| **LRU** | Least Recently Used | General purpose |
| **LFU** | Least Frequently Used | Power-law access |
| **FIFO** | First In First Out | Simple, streaming |
| **Random** | Random eviction | Simple, O(1) |
| **TTL** | Time-based expiry | Freshness critical |

### LRU Implementation

```
Doubly Linked List + HashMap

HashMap: key → node
List: most recent → least recent

Get(key):
  1. Find node in HashMap O(1)
  2. Move to front of list O(1)

Put(key, value):
  1. If exists: update, move to front
  2. If new: add to front
  3. If full: remove from back
```

---

## Distributed Caching

### Redis vs Memcached

| Feature | Redis | Memcached |
|---------|-------|-----------|
| Data types | Rich (strings, lists, sets, hashes) | Strings only |
| Persistence | Yes | No |
| Replication | Yes | No |
| Cluster mode | Yes | Yes (client-side) |
| Pub/Sub | Yes | No |
| Lua scripting | Yes | No |
| Memory efficiency | Good | Better |

**Use Redis for:**
- Complex data types
- Persistence needed
- Pub/sub functionality

**Use Memcached for:**
- Simple key-value
- Maximum memory efficiency
- Existing infrastructure

### Cache Cluster Architecture

```
┌─────────────────────────────────────────────┐
│                 Redis Cluster               │
│                                             │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐     │
│  │ Master 1│  │ Master 2│  │ Master 3│     │
│  │ (0-5460)│  │(5461-   │  │(10923-  │     │
│  └────┬────┘  │  10922) │  │  16383) │     │
│       │       └────┬────┘  └────┬────┘     │
│       │            │            │          │
│  ┌────▼────┐  ┌────▼────┐  ┌────▼────┐     │
│  │ Replica │  │ Replica │  │ Replica │     │
│  └─────────┘  └─────────┘  └─────────┘     │
└─────────────────────────────────────────────┘
```

Hash slots (16384 total) distributed across masters.

---

## Cache Patterns

### Cache Stampede Prevention

**Problem:** Cache expires → many requests hit DB simultaneously

**Solutions:**

1. **Locking:**
```
if cache_miss:
    if acquire_lock(key):
        value = load_from_db()
        set_cache(key, value)
        release_lock(key)
    else:
        wait_and_retry()
```

2. **Probabilistic early expiration:**
```
ttl_with_jitter = ttl * (0.8 + random() * 0.4)
```

3. **Background refresh:**
```
if ttl < threshold:
    trigger_async_refresh(key)
return cached_value
```

### Hot Key Problem

**Problem:** Один ключ получает много запросов → перегрузка

**Solutions:**
1. **Local cache:** Дополнительный in-memory cache
2. **Replicate hot keys:** Копии на разных нодах
3. **Sharding:** key_1, key_2, key_3 с round-robin

---

## Cache Metrics

### Key Metrics to Monitor

| Metric | Description | Target |
|--------|-------------|--------|
| Hit rate | Hits / (Hits + Misses) | > 90% |
| Latency (p99) | 99th percentile response | < 1ms |
| Memory usage | Used / Total | < 80% |
| Evictions | Items evicted | Low, stable |
| Connections | Active connections | Within limits |

### Cache Hit Rate Analysis

```
Low hit rate causes:
1. TTL too short
2. Working set > cache size
3. Random access pattern
4. Key design issues
```

---

## Best Practices

1. **Cache frequently read, rarely changed data**
2. **Set appropriate TTLs** — balance freshness vs load
3. **Monitor hit rates** — aim for 90%+
4. **Plan for cache failures** — graceful degradation
5. **Avoid caching large objects** — serialize/compress
6. **Use consistent hashing** — for distributed cache
7. **Warm up cache** — before traffic spikes

---

## Key Takeaways

1. **Cache-aside** — самый распространённый паттерн
2. **TTL** — простейший способ invalidation
3. **LRU** — универсальная eviction policy
4. **Redis** — для сложных типов данных
5. **Hit rate > 90%** — цель для production
6. **Hot keys** — распределяй или реплицируй
