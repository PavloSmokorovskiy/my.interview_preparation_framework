# START HERE: System Design за 5 минут

> **Время изучения:** ~5 минут (обзор), ~2-3 часа (полное изучение)

**Цель:** Понимать базовые концепции масштабируемых систем. Уметь структурировано отвечать на design questions.

---

## Что здесь есть

```
system-design/
│
├── START-HERE.md                    ← Ты здесь
├── COMPLETE-SYSTEM-DESIGN-GUIDE.md  ← ГЛАВНЫЙ ФАЙЛ
│
├── topics/                          ← Детальные гайды
│   ├── 01-fundamentals.md           ← Базовые концепции
│   ├── 02-databases.md              ← Базы данных и хранение
│   ├── 03-caching.md                ← Кэширование
│   ├── 04-load-balancing.md         ← Балансировка нагрузки
│   ├── 05-messaging.md              ← Очереди и messaging
│   └── 06-common-systems.md         ← Типовые системы (URL shortener и др.)
│
└── cheatsheets/
    ├── design-template.md           ← Шаблон для design interview
    └── quick-reference.md           ← Быстрая справка
```

---

## Структура System Design интервью

### Фреймворк ACED (15-20 минут на вопрос)

| Этап | Время | Что делать |
|------|-------|------------|
| **A**sk | 2-3 мин | Уточняющие вопросы, requirements |
| **C**onceptualize | 3-5 мин | High-level design, основные компоненты |
| **E**laborate | 5-8 мин | Детализация компонентов, trade-offs |
| **D**iscuss | 3-5 мин | Bottlenecks, масштабирование, мониторинг |

---

## Ключевые концепции — выучи

### Иерархия latency (знай порядки)

```
L1 cache:           ~1 ns
L2 cache:           ~10 ns
RAM:                ~100 ns
SSD random read:    ~100 μs (100,000 ns)
HDD seek:           ~10 ms
Network (same DC):  ~0.5 ms
Network (cross DC): ~100 ms
```

### CAP теорема

> В распределённой системе можно гарантировать только 2 из 3:
> - **C**onsistency — все узлы видят одинаковые данные
> - **A**vailability — система отвечает на каждый запрос
> - **P**artition tolerance — система работает при разрыве сети

**Практика:** При network partition выбираем CP (consistency) или AP (availability).

---

## 5 вопросов в начале любого design

1. **Users:** Сколько пользователей? DAU/MAU?
2. **Scale:** Сколько запросов в секунду (QPS)?
3. **Storage:** Сколько данных хранить? Retention?
4. **Latency:** Требования к скорости ответа?
5. **Availability:** 99.9%? 99.99%? Допустимый downtime?

---

## Базовые компоненты — когда какой

| Компонент | Когда использовать | Примеры |
|-----------|-------------------|---------|
| **Load Balancer** | Распределение нагрузки | nginx, HAProxy, AWS ALB |
| **Cache** | Частый read, редкий write | Redis, Memcached |
| **CDN** | Статический контент, geo-distributed users | CloudFront, Cloudflare |
| **Message Queue** | Async processing, decoupling | Kafka, RabbitMQ, SQS |
| **Database** | Persistent storage | PostgreSQL, MySQL, MongoDB |
| **Search** | Full-text search, facets | Elasticsearch, Solr |

---

## Типы баз данных — когда какую

| Тип | Когда использовать | Примеры |
|-----|-------------------|---------|
| **SQL (RDBMS)** | ACID, сложные queries, joins | PostgreSQL, MySQL |
| **NoSQL Document** | Flexible schema, JSON-like | MongoDB, DynamoDB |
| **NoSQL Key-Value** | Simple lookups, caching | Redis, DynamoDB |
| **NoSQL Wide-Column** | Time-series, write-heavy | Cassandra, HBase |
| **Graph** | Relationships, social networks | Neo4j, Neptune |

---

## Паттерны масштабирования

### Vertical vs Horizontal

| | Vertical (Scale Up) | Horizontal (Scale Out) |
|-|---------------------|------------------------|
| **Что** | Больше CPU/RAM на один сервер | Больше серверов |
| **Плюсы** | Просто, нет изменений в коде | Почти неограниченно |
| **Минусы** | Есть предел, single point of failure | Complexity, consistency |

### Database Scaling

1. **Read replicas** — для read-heavy workloads
2. **Sharding** — разделение данных по ключу
3. **Caching** — уменьшение нагрузки на DB

---

## Типовые системы — знай подходы

| Система | Ключевые компоненты | Challenges |
|---------|---------------------|------------|
| **URL Shortener** | Hash function, KV store | Collision handling, analytics |
| **Chat System** | WebSockets, message queue | Real-time delivery, presence |
| **News Feed** | Fan-out, caching, ranking | Scale, personalization |
| **Rate Limiter** | Token bucket, sliding window | Distributed counting |
| **Notification** | Message queue, push services | Delivery guarantees, batching |

---

## Приоритет изучения

### Must Know (задают часто)
1. **Load Balancing** — как распределять нагрузку
2. **Caching** — стратегии, invalidation
3. **Database** — SQL vs NoSQL, sharding, replication
4. **CAP теорема** — trade-offs в распределённых системах

### Good to Know
5. Message Queues — async processing
6. Microservices vs Monolith — когда что
7. API Design — REST, pagination, rate limiting

### Nice to Know
8. Consensus algorithms (Raft, Paxos)
9. Consistent hashing
10. Bloom filters

---

## Типичные ошибки

### 1. Сразу в детали
- **Плохо:** Начинать с выбора базы данных
- **Хорошо:** Сначала requirements, потом high-level design

### 2. Игнорировать масштаб
- **Плохо:** Design для 100 users = design для 100M users
- **Хорошо:** Спросить про scale, адаптировать решение

### 3. Один "правильный" ответ
- **Плохо:** "Нужно использовать X"
- **Хорошо:** "X подходит потому что... альтернатива Y, но..."

---

## Начни сейчас

1. Прочитай [`COMPLETE-SYSTEM-DESIGN-GUIDE.md`](COMPLETE-SYSTEM-DESIGN-GUIDE.md)
2. Выучи [`cheatsheets/design-template.md`](cheatsheets/design-template.md)
3. Разбери 2-3 типовые системы из `topics/06-common-systems.md`

**Удачи!**
