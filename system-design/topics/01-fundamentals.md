# System Design Fundamentals

> **Время изучения:** ~15 минут

Базовые концепции распределённых систем.

---

## CAP Theorem

### Определение

В распределённой системе невозможно одновременно гарантировать все три свойства:

- **C (Consistency):** Все узлы видят одинаковые данные в один момент времени
- **A (Availability):** Каждый запрос получает ответ (success или failure)
- **P (Partition Tolerance):** Система продолжает работать при разрыве сети между узлами

### Практическое применение

**При network partition (P всегда выбираем):**

| Выбор | Поведение | Примеры |
|-------|-----------|---------|
| **CP** | Отказываем в запросах до восстановления consistency | Банки, inventory |
| **AP** | Отвечаем, но данные могут быть stale | Social media, DNS |

### PACELC Theorem (расширение CAP)

> Если Partition, то выбор между Availability и Consistency.
> Else (нет partition), выбор между Latency и Consistency.

---

## ACID vs BASE

### ACID (Traditional SQL)

| Свойство | Описание |
|----------|----------|
| **Atomicity** | Транзакция выполняется полностью или не выполняется вообще |
| **Consistency** | Данные переходят из одного валидного состояния в другое |
| **Isolation** | Параллельные транзакции не влияют друг на друга |
| **Durability** | Завершённые транзакции сохранены навсегда |

### BASE (NoSQL approach)

| Свойство | Описание |
|----------|----------|
| **Basically Available** | Система в основном доступна |
| **Soft state** | Состояние может меняться со временем |
| **Eventual consistency** | Данные станут консистентными eventually |

### Когда что

| Сценарий | Подход |
|----------|--------|
| Финансовые транзакции | ACID |
| Social media posts | BASE |
| E-commerce inventory | ACID |
| User sessions | BASE |
| Payment processing | ACID |
| Analytics events | BASE |

---

## Latency Numbers

### Memory Hierarchy

```
┌─────────────────────────────────────────┐
│           L1 Cache: ~1 ns               │
│    ┌───────────────────────────────┐    │
│    │     L2 Cache: ~4 ns           │    │
│    │  ┌────────────────────────┐   │    │
│    │  │   L3 Cache: ~10 ns     │   │    │
│    │  │  ┌──────────────────┐  │   │    │
│    │  │  │  RAM: ~100 ns    │  │   │    │
│    │  │  └──────────────────┘  │   │    │
│    │  └────────────────────────┘   │    │
│    └───────────────────────────────┘    │
└─────────────────────────────────────────┘
```

### Storage & Network

| Операция | Latency | Сравнение |
|----------|---------|-----------|
| L1 cache | 1 ns | — |
| L2 cache | 4 ns | 4× L1 |
| RAM | 100 ns | 100× L1 |
| SSD random read | 16 μs | 16,000× L1 |
| HDD seek | 2 ms | 2,000,000× L1 |
| Network (same DC) | 0.5 ms | 500,000× L1 |
| Network (CA → EU) | 150 ms | 150,000,000× L1 |

### Правила оптимизации

1. **Кэшируй** — L1 > L2 > RAM > Disk
2. **Уменьшай round-trips** — особенно cross-DC
3. **Batch операции** — один запрос вместо многих
4. **Async, когда можно** — не блокируй на slow ops

---

## Consistency Models

### Strong Consistency

- Каждый read видит последний write
- Проще для разработчика
- Выше latency
- Ниже availability

### Eventual Consistency

- Reads могут вернуть stale данные
- Eventually все узлы синхронизируются
- Ниже latency
- Выше availability

### Causal Consistency

- Operations, связанные причинно, видны в правильном порядке
- Несвязанные operations могут быть в любом порядке
- Компромисс между strong и eventual

### Read-your-writes Consistency

- Пользователь всегда видит свои собственные writes
- Useful для user sessions

---

## Fault Tolerance Concepts

### Single Point of Failure (SPOF)

> Компонент, отказ которого приводит к отказу всей системы.

**Как устранить:**
- Redundancy (дублирование)
- Load balancing
- Failover mechanisms

### Failover Strategies

| Стратегия | Описание | Downtime |
|-----------|----------|----------|
| **Cold standby** | Backup запускается при failure | Минуты |
| **Warm standby** | Backup работает, но не принимает трафик | Секунды |
| **Hot standby** | Backup принимает трафик параллельно | Миллисекунды |

### Replication

**Synchronous:**
- Write подтверждается после записи на все replicas
- Strong consistency
- Higher latency

**Asynchronous:**
- Write подтверждается сразу, replication в background
- Eventual consistency
- Lower latency
- Возможна потеря данных при failure

---

## Throughput & Bandwidth

### Definitions

- **Throughput:** Количество операций в единицу времени
- **Bandwidth:** Максимальная пропускная способность канала
- **Latency:** Время выполнения одной операции

### Relationship

```
Throughput = Bandwidth / (Latency + Processing Time)
```

### Little's Law

```
L = λ × W

L = среднее количество items в системе
λ = arrival rate (requests per second)
W = среднее время обработки
```

**Пример:**
- 100 QPS, каждый запрос обрабатывается 0.5 сек
- L = 100 × 0.5 = 50 concurrent requests

---

## Back-of-the-Envelope Calculations

### Time Conversions

| Период | Секунды | Приближение |
|--------|---------|-------------|
| 1 минута | 60 | ~100 |
| 1 час | 3,600 | ~4K |
| 1 день | 86,400 | ~100K |
| 1 неделя | 604,800 | ~600K |
| 1 месяц | 2,592,000 | ~2.5M |
| 1 год | 31,536,000 | ~30M |

### Data Size Estimates

| Данные | Размер |
|--------|--------|
| ASCII char | 1 byte |
| UTF-8 char | 1-4 bytes |
| Integer | 4 bytes |
| Long | 8 bytes |
| UUID | 16 bytes |
| Timestamp | 8 bytes |
| Short text (tweet) | 300 bytes |
| Metadata record | 1 KB |
| Image (thumbnail) | 10 KB |
| Image (full) | 200 KB |
| Video (1 min, compressed) | 50 MB |

### Quick Math Tricks

```
Powers of 2:
2^10 = 1,024 ≈ 1 thousand (1K)
2^20 = 1,048,576 ≈ 1 million (1M)
2^30 = 1,073,741,824 ≈ 1 billion (1B)
2^40 ≈ 1 trillion (1T)

QPS from daily:
Daily requests / 100,000 ≈ QPS
(более точно: daily / 86,400)
```

---

## Пример расчёта

**Задача:** Twitter-like система

**Given:**
- 500M total users
- 200M DAU
- Each user: 2 tweets/day, 200 reads/day
- Tweet size: 300 bytes + metadata

**Calculate:**

1. **Write QPS:**
   - 200M × 2 tweets = 400M tweets/day
   - 400M / 100K ≈ 4K QPS
   - Peak (2× average): ~8K QPS

2. **Read QPS:**
   - 200M × 200 reads = 40B reads/day
   - 40B / 100K = 400K QPS
   - Peak: ~800K QPS

3. **Storage (per day):**
   - 400M tweets × 500 bytes = 200 GB/day
   - Per year: ~70 TB

4. **Read:Write ratio:**
   - 400K / 4K = 100:1
   - → Heavy caching needed!

---

## Key Takeaways

1. **CAP** — при partition выбираем CP или AP
2. **Latency** — memory > SSD > network > disk
3. **Consistency** — trade-off с availability и latency
4. **Calculations** — порядки величин важнее точных чисел
5. **Read:Write ratio** — определяет архитектуру
