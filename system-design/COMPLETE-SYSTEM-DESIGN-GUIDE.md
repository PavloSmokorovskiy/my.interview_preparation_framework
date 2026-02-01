# Complete System Design Guide

> **Время изучения:** ~45 минут на первое прочтение

Полное руководство по System Design для интервью в Google и другие tech компании.

---

## Содержание

1. [Фреймворк для интервью](#фреймворк-для-интервью)
2. [Фундаментальные концепции](#фундаментальные-концепции)
3. [Компоненты распределённых систем](#компоненты-распределённых-систем)
4. [Базы данных](#базы-данных)
5. [Масштабирование](#масштабирование)
6. [Типовые системы](#типовые-системы)
7. [Чек-лист для интервью](#чек-лист-для-интервью)

---

## Фреймворк для интервью

### ACED Framework

#### A — Ask (Уточняющие вопросы) | 2-3 минуты

**Функциональные требования:**
- Какие основные use cases?
- Кто пользователи? (B2C, B2B, internal)
- Какие операции поддерживаем? (CRUD, search, etc.)

**Нефункциональные требования:**
- **Scale:** DAU, MAU, QPS
- **Performance:** Latency requirements
- **Availability:** SLA, допустимый downtime
- **Consistency:** Strong vs eventual

**Constraints:**
- Бюджет? (startup vs enterprise)
- Existing infrastructure?
- Geographic distribution?

#### C — Conceptualize (High-level design) | 3-5 минут

1. Нарисуй основные компоненты
2. Покажи data flow
3. Определи API endpoints
4. Выбери storage (DB type, file storage)

#### E — Elaborate (Детализация) | 5-8 минут

1. Углубись в критические компоненты
2. Обсуди trade-offs решений
3. Покажи data model / schema
4. Адресуй bottlenecks

#### D — Discuss (Обсуждение) | 3-5 минут

1. Как масштабировать?
2. Что мониторить?
3. Failure scenarios
4. Future improvements

---

## Фундаментальные концепции

### CAP Теорема

```
        Consistency
           /\
          /  \
         /    \
        /  CA  \
       /________\
      AP        CP
Availability  Partition
              Tolerance
```

**В реальности:** При network partition выбираем:
- **CP (Consistency + Partition Tolerance):** Банковские системы, inventory
- **AP (Availability + Partition Tolerance):** Social media, caching

### ACID vs BASE

| ACID (SQL) | BASE (NoSQL) |
|------------|--------------|
| **A**tomicity | **B**asically **A**vailable |
| **C**onsistency | **S**oft state |
| **I**solation | **E**ventual consistency |
| **D**urability | |

### Latency Numbers (2024)

| Операция | Время |
|----------|-------|
| L1 cache reference | 1 ns |
| L2 cache reference | 4 ns |
| Main memory reference | 100 ns |
| SSD random read | 16 μs |
| HDD seek | 2 ms |
| Round trip same datacenter | 0.5 ms |
| Round trip CA → Netherlands | 150 ms |

### Throughput Estimates

| Metric | Estimate |
|--------|----------|
| QPS handled by web server | 1K-10K |
| QPS handled by database | 1K-10K |
| QPS handled by cache (Redis) | 100K+ |

---

## Компоненты распределённых систем

### 1. Load Balancer

**Что делает:** Распределяет трафик между серверами

**Алгоритмы:**
- **Round Robin** — по очереди
- **Least Connections** — к наименее загруженному
- **IP Hash** — по IP клиента (sticky sessions)
- **Weighted** — с учётом мощности серверов

**Layers:**
- **L4 (Transport):** TCP/UDP level, быстрее
- **L7 (Application):** HTTP level, умнее (content-based routing)

**Примеры:** nginx, HAProxy, AWS ALB/NLB

### 2. Caching

**Стратегии записи:**
- **Cache-aside:** App читает/пишет в cache и DB отдельно
- **Write-through:** Пишем в cache, cache пишет в DB
- **Write-behind:** Пишем в cache, cache async пишет в DB

**Eviction policies:**
- **LRU** (Least Recently Used) — самый популярный
- **LFU** (Least Frequently Used)
- **TTL** (Time To Live)

**Cache invalidation:**
- TTL-based
- Event-based (pub/sub)
- Manual purge

**Примеры:** Redis, Memcached

### 3. CDN (Content Delivery Network)

**Что кэшировать:**
- Статические файлы (images, CSS, JS)
- API responses (с осторожностью)

**Pull vs Push:**
- **Pull:** CDN запрашивает при первом обращении
- **Push:** Мы загружаем контент заранее

**Примеры:** CloudFront, Cloudflare, Akamai

### 4. Message Queue

**Когда использовать:**
- Async processing
- Decoupling сервисов
- Peak load handling
- Retry logic

**Модели:**
- **Point-to-Point:** Один consumer получает message
- **Pub/Sub:** Все subscribers получают message

**Гарантии доставки:**
- **At-most-once:** Может потеряться
- **At-least-once:** Может дублироваться
- **Exactly-once:** Сложно, но возможно (с idempotency)

**Примеры:** Kafka, RabbitMQ, AWS SQS

### 5. API Gateway

**Функции:**
- Routing
- Authentication/Authorization
- Rate limiting
- Request/Response transformation
- Caching
- Monitoring

**Примеры:** Kong, AWS API Gateway, nginx

---

## Базы данных

### SQL vs NoSQL

| Критерий | SQL | NoSQL |
|----------|-----|-------|
| Schema | Fixed | Flexible |
| Scaling | Vertical (обычно) | Horizontal |
| ACID | Да | Зависит |
| Joins | Эффективные | Обычно нет |
| Best for | Complex queries, transactions | Scale, flexibility |

### Типы NoSQL

| Тип | Use Case | Примеры |
|-----|----------|---------|
| **Key-Value** | Sessions, caching | Redis, DynamoDB |
| **Document** | Content management, catalogs | MongoDB, CouchDB |
| **Wide-Column** | Time-series, analytics | Cassandra, HBase |
| **Graph** | Social networks, recommendations | Neo4j, Neptune |

### Database Replication

**Master-Slave (Primary-Replica):**
```
Write → [Master] → Replication → [Slave 1]
                               → [Slave 2]
Read  ← [Slave 1]
Read  ← [Slave 2]
```

- Writes идут в Master
- Reads идут в Slaves
- Replication lag → eventual consistency

**Master-Master:**
- Оба принимают writes
- Conflict resolution нужен
- Сложнее в управлении

### Database Sharding

**Horizontal Sharding (Partitioning):**

| Стратегия | Описание | Плюсы | Минусы |
|-----------|----------|-------|--------|
| **Range-based** | По диапазону ключа | Простота | Hotspots |
| **Hash-based** | По hash ключа | Равномерность | Сложность range queries |
| **Directory-based** | Lookup table | Flexibility | Single point of failure |

**Consistent Hashing:**
- Минимизирует перераспределение при добавлении/удалении узлов
- Используется в distributed caches, databases

### Indexing

**Типы индексов:**
- **B-Tree:** Универсальный, range queries
- **Hash:** Equality queries, O(1)
- **Full-text:** Текстовый поиск
- **Composite:** Несколько колонок

**Trade-offs:**
- Ускоряют reads
- Замедляют writes
- Занимают место

---

## Масштабирование

### Vertical vs Horizontal

| | Vertical Scaling | Horizontal Scaling |
|-|-----------------|-------------------|
| **Как** | Больше RAM/CPU | Больше машин |
| **Limit** | Hardware limit | Практически нет |
| **Complexity** | Низкая | Высокая |
| **Cost** | Дорого на high-end | Линейно |
| **Downtime** | При upgrade | Нет (с LB) |

### Stateless vs Stateful

**Stateless:**
- Сервер не хранит session state
- Любой сервер может обработать запрос
- Легко масштабировать

**Stateful:**
- Сервер хранит session state
- Sticky sessions или shared storage нужен
- Сложнее масштабировать

### Rate Limiting

**Алгоритмы:**
- **Token Bucket:** Токены накапливаются, тратятся на запросы
- **Leaky Bucket:** Запросы обрабатываются с фиксированной скоростью
- **Fixed Window:** Счётчик сбрасывается каждый период
- **Sliding Window:** Более точный, но сложнее

**Где:**
- API Gateway level
- Application level
- Database level

### Back-of-the-Envelope Calculations

**Полезные числа:**
- 1 день = 86,400 секунд ≈ 100K секунд
- 1 миллион запросов/день ≈ 12 QPS
- 1 миллиард запросов/день ≈ 12K QPS

**Пример расчёта:**
```
Twitter-like system:
- 500M users, 100M DAU
- Each user: 2 tweets/day, 100 reads/day

Writes: 100M × 2 = 200M/day ≈ 2.3K QPS
Reads: 100M × 100 = 10B/day ≈ 115K QPS

Read:Write ratio ≈ 50:1 → Cache heavily!
```

---

## Типовые системы

### URL Shortener (TinyURL)

**Requirements:**
- Shorten URL
- Redirect to original
- Analytics (optional)

**Key decisions:**
- **ID generation:** Counter, hash, random
- **Storage:** Key-value store
- **Collision handling:** Check & retry

**Components:**
```
Client → API Gateway → App Server → Cache → Database
                                      ↑
                                  Key: shortURL
                                  Value: longURL
```

### Chat System

**Requirements:**
- 1-on-1 chat
- Group chat
- Online status
- Push notifications

**Key decisions:**
- **Protocol:** WebSocket for real-time
- **Message storage:** Per-conversation or per-user
- **Delivery:** At-least-once with deduplication

**Components:**
```
Client ←→ WebSocket Server ←→ Message Queue ←→ Chat Service ←→ DB
                                    ↓
                              Notification Service
```

### News Feed (Facebook/Twitter)

**Requirements:**
- Post creation
- Feed generation
- Ranking

**Approaches:**
- **Pull (Fan-out on read):** Generate feed on request
- **Push (Fan-out on write):** Pre-compute feed on post

**Hybrid approach:**
- Push for most users
- Pull for celebrities (many followers)

### Rate Limiter

**Requirements:**
- Limit requests per user/IP
- Distributed (multiple servers)
- Low latency

**Key decisions:**
- **Algorithm:** Token bucket most common
- **Storage:** Redis for distributed counting
- **Granularity:** Per second, minute, hour

---

## Чек-лист для интервью

### Перед началом
- [ ] Уточнил functional requirements
- [ ] Уточнил non-functional requirements (scale, latency, availability)
- [ ] Понял constraints и assumptions

### High-level design
- [ ] Нарисовал основные компоненты
- [ ] Показал data flow
- [ ] Определил API (endpoints, methods)
- [ ] Выбрал storage решения

### Deep dive
- [ ] Детализировал критические компоненты
- [ ] Обсудил trade-offs решений
- [ ] Показал data model
- [ ] Адресовал bottlenecks

### Масштабирование
- [ ] Обсудил horizontal scaling
- [ ] Рассмотрел caching strategy
- [ ] Предложил database scaling (replication, sharding)
- [ ] Учёл geographic distribution (если нужно)

### Надёжность
- [ ] Обсудил failure scenarios
- [ ] Предложил monitoring/alerting
- [ ] Рассмотрел disaster recovery

---

## Ресурсы для углубления

### Книги
- "Designing Data-Intensive Applications" by Martin Kleppmann
- "System Design Interview" by Alex Xu

### Online
- [System Design Primer](https://github.com/donnemartin/system-design-primer)
- [Grokking System Design](https://www.educative.io/courses/grokking-the-system-design-interview)

---

**Помни:** System Design — это не про "правильный ответ", а про structured thinking и обсуждение trade-offs.
