# Load Balancing

> **Время изучения:** ~10 минут

Распределение нагрузки между серверами.

---

## Зачем Load Balancer

- **Распределение трафика:** Равномерная нагрузка
- **High availability:** Отказоустойчивость
- **Horizontal scaling:** Добавление серверов
- **Health checks:** Отключение нездоровых серверов
- **SSL termination:** Централизованная обработка HTTPS

---

## Types of Load Balancers

### Layer 4 (Transport)

```
┌────────────┐
│     L4     │ ─── Работает с TCP/UDP
│     LB     │ ─── Быстрее (меньше processing)
└────────────┘ ─── Не видит HTTP content
```

**Routing based on:**
- Source/Destination IP
- Source/Destination Port
- Protocol (TCP/UDP)

### Layer 7 (Application)

```
┌────────────┐
│     L7     │ ─── Работает с HTTP
│     LB     │ ─── Умнее (content-based routing)
└────────────┘ ─── Выше latency
```

**Routing based on:**
- URL path
- HTTP headers
- Cookies
- Query parameters
- Request body

### Comparison

| Feature | L4 | L7 |
|---------|----|----|
| Speed | Faster | Slower |
| Intelligence | Less | More |
| SSL termination | No | Yes |
| Content caching | No | Yes |
| Path-based routing | No | Yes |
| WebSocket support | Limited | Full |

---

## Load Balancing Algorithms

### 1. Round Robin

```
Request 1 → Server A
Request 2 → Server B
Request 3 → Server C
Request 4 → Server A (cycle)
```

**Плюсы:** Простота, равномерность
**Минусы:** Игнорирует нагрузку серверов

### 2. Weighted Round Robin

```
Server A (weight 3): 3 requests
Server B (weight 2): 2 requests
Server C (weight 1): 1 request
```

**Use case:** Серверы разной мощности

### 3. Least Connections

```
Current connections:
Server A: 10
Server B: 5  ← Next request goes here
Server C: 8
```

**Плюсы:** Учитывает текущую нагрузку
**Минусы:** Не учитывает время обработки

### 4. Least Response Time

```
Avg response time:
Server A: 100ms
Server B: 50ms  ← Next request goes here
Server C: 75ms
```

**Плюсы:** Оптимальная производительность
**Минусы:** Сложнее реализовать

### 5. IP Hash

```
hash(client_ip) % num_servers = server_index
```

**Плюсы:** Sticky sessions без cookies
**Минусы:** Неравномерность при hash collisions

### 6. Consistent Hashing

```
Hash Ring:
     Server A
        │
   ┌────┴────┐
   │         │
Server D   Server B
   │         │
   └────┬────┘
        │
     Server C

request_key → closest server on ring
```

**Плюсы:** Минимальное перераспределение
**Минусы:** Сложность

---

## Health Checks

### Types

| Type | Method | Use case |
|------|--------|----------|
| **TCP** | Try connect | Basic availability |
| **HTTP** | GET /health | Application health |
| **Custom** | Script/command | Complex checks |

### Health Check Parameters

```yaml
health_check:
  interval: 10s        # Check every 10 seconds
  timeout: 5s          # Wait 5 seconds for response
  unhealthy_threshold: 3  # 3 failures = unhealthy
  healthy_threshold: 2    # 2 successes = healthy
```

### Graceful Degradation

```
┌──────────┐
│    LB    │
└────┬─────┘
     │
  ┌──┴───┬────────┐
  ▼      ▼        ▼
[OK]  [FAIL]    [OK]
 ✓      ✗        ✓
 │               │
 └───────────────┘
   Traffic only to healthy servers
```

---

## Session Persistence (Sticky Sessions)

### Problem

```
Request 1 → Server A (login, create session)
Request 2 → Server B (no session!)
```

### Solutions

| Method | How it works | Pros | Cons |
|--------|--------------|------|------|
| **Cookie-based** | LB adds server ID to cookie | Simple | Cookie overhead |
| **IP Hash** | Route by client IP | No cookies | NAT issues |
| **Shared session store** | Redis/Memcached | Stateless servers | Extra hop |
| **JWT tokens** | Stateless auth | No server affinity | Token size |

### Recommendation

**Prefer stateless design:**
- Store sessions in Redis
- Any server can handle any request
- Better scalability

---

## High Availability

### Active-Passive

```
         ┌──────────────┐
Traffic ─▶│  Active LB  │
         └──────┬───────┘
                │ Heartbeat
         ┌──────▼───────┐
         │  Passive LB  │ (standby)
         └──────────────┘

If Active fails:
- Passive takes over
- Same virtual IP
```

### Active-Active

```
              ┌───────────┐
         ┌───▶│   LB 1    │
Traffic ─┤    └───────────┘
(DNS)    │
         │    ┌───────────┐
         └───▶│   LB 2    │
              └───────────┘
```

- DNS returns multiple IPs
- Both LBs active
- Better utilization

---

## Common Load Balancers

### Software

| Name | Type | Features |
|------|------|----------|
| **nginx** | L7 | HTTP, reverse proxy, caching |
| **HAProxy** | L4/L7 | High performance, TCP/HTTP |
| **Envoy** | L7 | Service mesh, gRPC, observability |
| **Traefik** | L7 | Docker/K8s native, auto-discovery |

### Cloud

| Provider | Service |
|----------|---------|
| AWS | ALB (L7), NLB (L4), ELB Classic |
| GCP | Cloud Load Balancing |
| Azure | Azure Load Balancer |

---

## Architecture Patterns

### Single LB

```
┌────────┐     ┌────┐     ┌────────────┐
│ Client │────▶│ LB │────▶│  Servers   │
└────────┘     └────┘     └────────────┘
```

**Issue:** LB is SPOF

### LB with HA

```
                ┌──────┐
         ┌─────▶│ LB 1 │─────┐
┌────────┤      └──────┘     │    ┌─────────┐
│ Client │   Floating IP     ├───▶│ Servers │
└────────┤      ┌──────┐     │    └─────────┘
         └─────▶│ LB 2 │─────┘
                └──────┘
```

### Global Load Balancing

```
              ┌─────────────┐
              │  Global LB  │ (GeoDNS / Anycast)
              └──────┬──────┘
                     │
    ┌────────────────┼────────────────┐
    ▼                ▼                ▼
┌───────┐       ┌───────┐       ┌───────┐
│US-East│       │EU-West│       │AP-East│
│  LB   │       │  LB   │       │  LB   │
└───┬───┘       └───┬───┘       └───┬───┘
    │               │               │
┌───▼───┐       ┌───▼───┐       ┌───▼───┐
│Servers│       │Servers│       │Servers│
└───────┘       └───────┘       └───────┘
```

---

## Best Practices

1. **Use health checks** — route only to healthy servers
2. **Enable logging** — for debugging and analytics
3. **Set timeouts** — prevent hung connections
4. **Plan for failure** — HA configuration
5. **Monitor closely** — latency, errors, throughput
6. **Prefer stateless** — easier scaling
7. **Start with L7** — more flexibility

---

## Key Takeaways

1. **L4** быстрее, **L7** умнее
2. **Least connections** — хороший default
3. **Health checks** — обязательны
4. **Sticky sessions** — избегай, если можно
5. **HA** — active-passive или active-active
6. **Stateless** — лучшая архитектура для scaling
