# Databases & Storage

> **Время изучения:** ~15 минут

Выбор и масштабирование баз данных.

---

## SQL vs NoSQL

### Сравнение

| Критерий | SQL (RDBMS) | NoSQL |
|----------|-------------|-------|
| Schema | Фиксированная | Гибкая |
| Scaling | Vertical (обычно) | Horizontal |
| ACID | Да | Зависит от типа |
| Joins | Эффективные | Обычно нет |
| Query language | SQL (стандарт) | Разный |
| Best for | Complex queries, transactions | Scale, flexibility |

### Когда SQL

- Сложные транзакции (банки, e-commerce)
- Сложные joins и queries
- Data integrity критична
- Структура данных известна и стабильна

### Когда NoSQL

- Высокий объём данных
- Горизонтальное масштабирование
- Гибкая/меняющаяся схема
- Простые query patterns

---

## Типы NoSQL

### 1. Key-Value Stores

**Модель:** Простая пара ключ-значение

```
Key: "user:123"
Value: "{name: 'John', email: 'john@example.com'}"
```

| Примеры | Use Cases |
|---------|-----------|
| Redis | Caching, sessions, rate limiting |
| DynamoDB | High-scale key lookups |
| Memcached | Simple caching |

**Операции:** GET, SET, DELETE — O(1)

### 2. Document Stores

**Модель:** JSON-like документы

```json
{
  "_id": "123",
  "name": "John",
  "orders": [
    {"id": "o1", "total": 100},
    {"id": "o2", "total": 200}
  ]
}
```

| Примеры | Use Cases |
|---------|-----------|
| MongoDB | Content management, catalogs |
| CouchDB | Mobile sync |
| Firestore | Real-time apps |

**Плюсы:** Flexible schema, nested data
**Минусы:** No joins, eventual consistency

### 3. Wide-Column Stores

**Модель:** Таблицы с динамическими колонками

```
Row Key: "user:123"
Column Families:
  - profile: {name: "John", age: 30}
  - activity: {last_login: "2024-01-01", posts: 50}
```

| Примеры | Use Cases |
|---------|-----------|
| Cassandra | Time-series, IoT, logs |
| HBase | Analytics, Hadoop integration |
| ScyllaDB | High-performance Cassandra |

**Плюсы:** Excellent write performance, horizontal scale
**Минусы:** Limited query flexibility

### 4. Graph Databases

**Модель:** Nodes и Edges (relationships)

```
(User:John)-[:FOLLOWS]->(User:Jane)
(User:John)-[:LIKES]->(Post:123)
```

| Примеры | Use Cases |
|---------|-----------|
| Neo4j | Social networks, recommendations |
| Amazon Neptune | Knowledge graphs |
| JanusGraph | Large-scale graphs |

**Плюсы:** Efficient traversals, relationship queries
**Минусы:** Not for simple CRUD

---

## Database Scaling

### Vertical Scaling (Scale Up)

```
┌──────────────────────┐
│   Bigger Server      │
│   More CPU, RAM      │
│   Faster Disks       │
└──────────────────────┘
```

**Плюсы:** Простота
**Минусы:** Есть предел, дорого, SPOF

### Horizontal Scaling (Scale Out)

#### 1. Read Replicas

```
              ┌─────────────┐
   Writes ───▶│   Primary   │
              └──────┬──────┘
                     │ Replication
         ┌───────────┼───────────┐
         ▼           ▼           ▼
    ┌─────────┐ ┌─────────┐ ┌─────────┐
    │Replica 1│ │Replica 2│ │Replica 3│
    └────┬────┘ └────┬────┘ └────┬────┘
         │           │           │
         └───────────┴───────────┘
                     │
   Reads ◀───────────┘
```

- Writes идут в Primary
- Reads распределяются по Replicas
- Replication lag → eventual consistency

#### 2. Sharding (Partitioning)

```
┌─────────────┐
│   Router    │
└──────┬──────┘
       │
   ┌───┴───┬───────┬───────┐
   ▼       ▼       ▼       ▼
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│Shard│ │Shard│ │Shard│ │Shard│
│ A-F │ │ G-L │ │ M-R │ │ S-Z │
└─────┘ └─────┘ └─────┘ └─────┘
```

**Sharding Strategies:**

| Стратегия | Описание | Плюсы | Минусы |
|-----------|----------|-------|--------|
| Range | По диапазону ключа | Simple, range queries | Hotspots |
| Hash | По hash ключа | Even distribution | No range queries |
| Directory | Lookup table | Flexible | Extra hop |
| Geographic | По региону | Low latency | Complexity |

**Challenges:**
- Cross-shard queries
- Rebalancing
- Hotspots
- Joins across shards

---

## Replication Strategies

### Synchronous vs Asynchronous

| | Synchronous | Asynchronous |
|-|-------------|--------------|
| Consistency | Strong | Eventual |
| Latency | Higher | Lower |
| Durability | Guaranteed | Risk of loss |
| Throughput | Lower | Higher |

### Replication Topologies

**Single-Leader:**
```
    [Leader]
       │
   ┌───┼───┐
   ▼   ▼   ▼
  [F] [F] [F]
```
- All writes to leader
- Simple conflict resolution
- Leader is bottleneck

**Multi-Leader:**
```
[Leader A] ◄───► [Leader B]
    │                │
    ▼                ▼
   [F]              [F]
```
- Writes to any leader
- Better availability
- Conflict resolution needed

**Leaderless:**
```
[Node] ◄─► [Node]
   │    ╲ ╱    │
   ▼     ╳     ▼
[Node] ◄─► [Node]
```
- Write to multiple nodes
- Quorum for consistency
- DynamoDB, Cassandra style

---

## Indexing

### B-Tree Index

```
        [50]
       /    \
    [25]    [75]
   /  \    /   \
 [10][30][60][90]
```

- Balanced tree structure
- O(log n) search, insert, delete
- Range queries efficient
- Most common index type

### Hash Index

```
hash("key") → bucket → value
```

- O(1) lookup
- Equality queries only
- No range queries
- Used in key-value stores

### Composite Index

```sql
CREATE INDEX idx ON users(country, city, name);
```

- Multiple columns
- Order matters (leftmost prefix)
- Efficient for combined queries

### Full-Text Index

- For text search
- Inverted index structure
- Tokenization, stemming
- Elasticsearch, PostgreSQL FTS

---

## Consistency Patterns

### Read-Your-Writes

Пользователь всегда видит свои собственные writes.

**Implementation:**
- Read from leader after write
- Track user's last write timestamp

### Monotonic Reads

Пользователь не видит "откат" во времени.

**Implementation:**
- Sticky sessions to same replica
- Or track last read timestamp

### Consistent Prefix Reads

Если A произошло до B, читаем A перед B.

**Implementation:**
- Causal ordering
- Vector clocks

---

## Database Selection Guide

```
Start here:
    │
    ▼
Need ACID transactions?
    │
   Yes ──► SQL (PostgreSQL, MySQL)
    │
   No
    │
    ▼
What's the data model?
    │
    ├─► Key-Value ──► Redis, DynamoDB
    │
    ├─► Documents ──► MongoDB
    │
    ├─► Time-series ──► Cassandra, InfluxDB
    │
    ├─► Relationships ──► Neo4j
    │
    └─► Search ──► Elasticsearch
```

---

## Key Takeaways

1. **SQL** для транзакций и сложных queries
2. **NoSQL** для scale и flexibility
3. **Read replicas** для read-heavy workloads
4. **Sharding** когда одного сервера мало
5. **Indexes** ускоряют reads, замедляют writes
6. **Consistency** — trade-off с performance
