# Messaging & Queues

> **Время изучения:** ~10 минут

Асинхронная коммуникация между сервисами.

---

## Зачем Message Queues

- **Decoupling:** Сервисы независимы
- **Async processing:** Не блокировать пользователя
- **Peak handling:** Сглаживание нагрузки
- **Reliability:** Retry при failures
- **Scale:** Independent scaling producers/consumers

---

## Messaging Patterns

### 1. Point-to-Point (Queue)

```
┌──────────┐     ┌─────────┐     ┌──────────┐
│ Producer │────▶│  Queue  │────▶│ Consumer │
└──────────┘     └─────────┘     └──────────┘
```

- Один consumer получает сообщение
- Work distribution
- Examples: Task queues, job processing

### 2. Publish-Subscribe (Topic)

```
┌──────────┐     ┌─────────┐     ┌────────────┐
│ Publisher│────▶│  Topic  │────▶│ Subscriber1│
└──────────┘     └────┬────┘     └────────────┘
                      │
                      └─────────▶┌────────────┐
                                 │ Subscriber2│
                                 └────────────┘
```

- Все subscribers получают копию
- Event broadcasting
- Examples: Notifications, event sourcing

### 3. Request-Reply

```
┌──────────┐  request   ┌─────────┐  request   ┌──────────┐
│ Requester│───────────▶│  Queue  │───────────▶│ Replier  │
└────┬─────┘            └─────────┘            └─────┬────┘
     │                                               │
     │           reply              reply            │
     └◀────────────────────────────────────────────┘
```

- Async RPC pattern
- Correlation ID для matching

---

## Delivery Guarantees

### At-Most-Once

```
Producer → Queue → Consumer
             │
        (может потеряться)
```

- Fire and forget
- No retries
- Use case: Metrics, logs (где потеря OK)

### At-Least-Once

```
Producer → Queue → Consumer → Ack
             │         │
             └─ retry ─┘ (если нет ack)
```

- Может быть дублирование
- Требует idempotent consumers
- Most common

### Exactly-Once

```
Producer → Queue → Consumer
   │                  │
   └── idempotent ────┘
       + deduplication
```

- Сложно достичь
- Обычно: at-least-once + idempotency

---

## Message Queue Systems

### Apache Kafka

```
┌─────────────────────────────────────────┐
│              Kafka Cluster              │
│                                         │
│  Topic: orders                          │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │Partition│ │Partition│ │Partition│   │
│  │    0    │ │    1    │ │    2    │   │
│  └─────────┘ └─────────┘ └─────────┘   │
│                                         │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │ Broker1 │ │ Broker2 │ │ Broker3 │   │
│  └─────────┘ └─────────┘ └─────────┘   │
└─────────────────────────────────────────┘
```

**Features:**
- High throughput (millions msgs/sec)
- Durable (disk persistence)
- Scalable (partitions)
- Ordered within partition
- Consumer groups

**Use cases:**
- Event streaming
- Log aggregation
- Real-time analytics
- Event sourcing

### RabbitMQ

```
┌─────────────────────────────────────────┐
│            RabbitMQ Broker              │
│                                         │
│  ┌──────────┐    ┌─────────┐            │
│  │ Exchange │───▶│  Queue  │────▶Consumer│
│  │ (routing)│    └─────────┘            │
│  └──────────┘                           │
│       │          ┌─────────┐            │
│       └─────────▶│  Queue  │────▶Consumer│
│                  └─────────┘            │
└─────────────────────────────────────────┘
```

**Features:**
- Flexible routing (exchanges)
- Multiple protocols (AMQP, MQTT, STOMP)
- Priority queues
- Dead letter queues
- Message acknowledgments

**Use cases:**
- Task queues
- RPC
- Complex routing
- Microservices communication

### Amazon SQS

**Standard Queue:**
- At-least-once delivery
- Best-effort ordering
- Unlimited throughput

**FIFO Queue:**
- Exactly-once processing
- Strict ordering
- 3000 msgs/sec (with batching)

**Use cases:**
- AWS-native applications
- Serverless (Lambda triggers)
- Simple queue needs

### Comparison

| Feature | Kafka | RabbitMQ | SQS |
|---------|-------|----------|-----|
| Throughput | Very high | High | High |
| Ordering | Partition | Queue | FIFO only |
| Persistence | Disk | Memory/Disk | Managed |
| Replay | Yes | No | No |
| Routing | Limited | Flexible | Limited |
| Management | Complex | Medium | Easy |

---

## Key Concepts

### Consumer Groups

```
Topic: orders (3 partitions)

Consumer Group A:
  Consumer 1 ← Partition 0
  Consumer 2 ← Partition 1
  Consumer 3 ← Partition 2

Consumer Group B:
  Consumer 4 ← All partitions
```

- Each partition consumed by one consumer in group
- Parallel processing
- Scale consumers ≤ partitions

### Dead Letter Queue (DLQ)

```
┌─────────┐     ┌───────┐     ┌──────────┐
│ Message │────▶│ Queue │────▶│ Consumer │
└─────────┘     └───────┘     └────┬─────┘
                                   │
                    Failed after   │
                    N retries      │
                                   ▼
                              ┌─────────┐
                              │   DLQ   │
                              └─────────┘
```

- Хранит failed messages
- Для debugging и reprocessing
- Alert on DLQ growth

### Backpressure

```
Producer (fast) ─────▶ Queue ─────▶ Consumer (slow)
                         │
                    Buffer grows!
```

**Solutions:**
1. **Drop messages** — если OK потерять
2. **Block producer** — backpressure to source
3. **Scale consumers** — увеличить throughput
4. **Rate limit** — ограничить producer

---

## Message Design

### Message Structure

```json
{
  "id": "uuid",
  "type": "order.created",
  "timestamp": "2024-01-01T00:00:00Z",
  "source": "order-service",
  "data": {
    "order_id": "123",
    "customer_id": "456",
    "total": 99.99
  },
  "metadata": {
    "correlation_id": "abc",
    "trace_id": "xyz"
  }
}
```

### Idempotency

```
Consumer receives same message twice:
Message ID: "msg-123"

Processing:
1. Check if "msg-123" already processed
2. If yes: skip (idempotent)
3. If no: process, mark as processed
```

**Implementation:**
- Store processed message IDs
- Use idempotency key in database
- Idempotent operations (PUT vs POST)

### Ordering

**When ordering matters:**
- Events for same entity (user actions)
- State machine transitions
- Financial transactions

**How to ensure:**
- Same partition/queue for related messages
- Sequence numbers
- Single consumer per partition

---

## Patterns

### Saga Pattern

Distributed transactions через events:

```
1. Order Service: Create Order
        │
        ▼ OrderCreated event
2. Payment Service: Process Payment
        │
        ▼ PaymentCompleted event
3. Inventory Service: Reserve Items
        │
        ▼ ItemsReserved event
4. Order Service: Complete Order

If any fails: Compensating transactions
```

### Event Sourcing

Store events, not state:

```
Events for Order 123:
1. OrderCreated { items: [...] }
2. PaymentReceived { amount: 100 }
3. ItemShipped { tracking: "ABC" }
4. OrderDelivered { date: "..." }

Current state = replay all events
```

### CQRS (Command Query Responsibility Segregation)

```
Commands (writes) → Event Store → Events
                                     │
                          ┌──────────┼──────────┐
                          ▼          ▼          ▼
                     [Read DB 1] [Read DB 2] [Read DB 3]
                          │
Queries (reads) ◀─────────┘
```

---

## Best Practices

1. **Idempotent consumers** — handle duplicates
2. **Set message TTL** — avoid queue buildup
3. **Use DLQ** — don't lose failed messages
4. **Monitor lag** — consumer behind producer
5. **Schema versioning** — backwards compatibility
6. **Correlation IDs** — tracing across services
7. **Batch when possible** — improve throughput

---

## Key Takeaways

1. **Queue** для work distribution, **Topic** для broadcasting
2. **At-least-once** + idempotency = practical exactly-once
3. **Kafka** для высокого throughput и replay
4. **RabbitMQ** для сложного routing
5. **DLQ** для handling failures
6. **Consumer groups** для parallel processing
