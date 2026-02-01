# Interview Preparation Framework

> **Общее время подготовки:** ~1-2 недели интенсивной подготовки

Полный фреймворк подготовки к интервью в Google для Software Engineer.

---

## Три раздела

### 📋 [Behavioral (Googleyness & Leadership)](behavioral/START-HERE.md)
Подготовка к behavioral раунду: истории, ответы на вопросы, STAR метод.

### 💻 [Algorithms & Data Structures](algorithms/START-HERE.md)
Алгоритмы и структуры данных: определения, сложности, когда использовать.

### 🏗️ [System Design](system-design/START-HERE.md)
Проектирование масштабируемых систем: компоненты, паттерны, типовые системы.

### 📅 [Недельный план подготовки](WEEKLY-PLAN.md)
Структурированный план на 1-2 недели с ежедневными задачами.

---

## Быстрый старт

### Behavioral (< 1 недели)

```
behavioral/
├── START-HERE.md               ← Начни здесь
├── COMPLETE-ANSWERS-GUIDE.md   ← 50+ готовых ответов (ГЛАВНЫЙ ФАЙЛ)
├── cheatsheets/
│   ├── master-cheatsheet.md    ← Всё на 3 страницах
│   └── stories-compressed.md   ← 6 историй для заучивания
├── interview-day/
│   ├── quick-review.md         ← Для утра интервью
│   └── pocket-cards.md         ← Карточки для печати
└── self-test.md                ← 50 вопросов самопроверки
```

> **Source of Truth:** `COMPLETE-ANSWERS-GUIDE.md` — главный источник ответов.

### Algorithms

```
algorithms/
├── START-HERE.md                    ← Начни здесь
├── COMPLETE-ALGORITHMS-GUIDE.md     ← Все алгоритмы (ГЛАВНЫЙ ФАЙЛ)
├── LEETCODE-PRACTICE.md             ← 75+ задач для практики (NEW!)
├── by-category/                     ← Детальные гайды (13 категорий)
├── data-structures/                 ← Структуры данных (7 файлов)
├── cheatsheets/
│   ├── big-o-cheatsheet.md         ← Сложности
│   ├── pattern-recognition.md      ← Как выбрать алгоритм
│   └── quick-reference.md          ← Краткая справка
└── self-test.md                     ← 50 вопросов самопроверки
```

### System Design

```
system-design/
├── START-HERE.md                    ← Начни здесь
├── COMPLETE-SYSTEM-DESIGN-GUIDE.md  ← Полное руководство (ГЛАВНЫЙ ФАЙЛ)
├── topics/                          ← Детальные гайды
│   ├── 01-fundamentals.md          ← CAP, ACID, latency
│   ├── 02-databases.md             ← SQL vs NoSQL, sharding
│   ├── 03-caching.md               ← Redis, стратегии
│   ├── 04-load-balancing.md        ← L4/L7, алгоритмы
│   ├── 05-messaging.md             ← Kafka, RabbitMQ
│   └── 06-common-systems.md        ← URL shortener, Chat и др.
└── cheatsheets/
    ├── design-template.md          ← Шаблон для интервью
    └── quick-reference.md          ← Быстрая справка
```

---

## Googleyness & Leadership (краткий обзор)

### 8 черт Googleyness
**Мнемоника: A-H-B-D-O-H-I-C**

1. **A**mbiguity — комфорт с неопределённостью
2. **H**umility — скромность и открытость к фидбеку
3. **B**ias for Action — ориентация на действия
4. **D**oing Right — этичное принятие решений
5. **O**wnership — ответственность
6. **H**igh Standards — высокие стандарты
7. **I**nnovation — креативное мышление
8. **C**ollaboration — командная работа

### 5 Leadership сигналов
**Мнемоника: I-I-M-R-D**

1. **I**nitiative — инициативность
2. **I**nfluence — влияние без власти
3. **M**entorship — развитие других
4. **R**esults — достижение результатов
5. **D**ecisions — принятие решений

### 6 историй (one-liners)

| # | Категория | One-liner |
|---|-----------|-----------|
| 1 | Conflict | Hybrid архитектура через данные + понимание concerns |
| 2 | Leadership + Mentorship | Frontend миграция 70%/4мес + обучил команды |
| 3 | Failure (technical) | Production incident → ownership → процессы |
| 4 | Ambiguity | MVP 6 недель + flexible architecture + weekly demos |
| 5 | Achievement | Cloud costs -$200K через quick wins |
| 6 | Failure (backup) | 2 мес на wrong feature → customer validation процесс |

---

## Algorithms (краткий обзор)

### Иерархия сложностей
```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ) < O(n!)
```

### Когда какой алгоритм

| Ситуация | Алгоритм |
|----------|----------|
| Sorted + search | Binary Search |
| Shortest path (unweighted) | BFS |
| Shortest path (weighted ≥0) | Dijkstra |
| All possibilities | Backtracking |
| Optimal with subproblems | DP |
| Local = Global optimal | Greedy |
| Contiguous subarray | Sliding Window |
| Pair in sorted array | Two Pointers |
| Min/max dynamically | Heap |
| Connected components | Union-Find |

### Основные структуры данных

| Структура | Операции | Когда использовать |
|-----------|----------|-------------------|
| HashMap | O(1) lookup | Быстрый поиск по ключу |
| Heap | O(1) min/max | Priority queue |
| BST | O(log n) sorted | Упорядоченные данные |
| Trie | O(m) prefix | Autocomplete, prefixes |
| Union-Find | O(α(n)) | Связные компоненты |

---

## Структура проекта

```
interview_preparation_framework/
│
├── README.md                        ← Ты здесь
│
├── behavioral/                      ← Googleyness & Leadership
│   ├── START-HERE.md
│   ├── COMPLETE-ANSWERS-GUIDE.md
│   ├── GOOGLEYNESS-DETAILED.md
│   ├── LEADERSHIP-DETAILED.md
│   ├── cheatsheets/
│   ├── interview-day/
│   ├── self-test.md
│   ├── question-mapping.md
│   ├── framework/
│   ├── questions/
│   ├── stories/
│   └── practice/
│
├── algorithms/                      ← Алгоритмы и структуры данных
│   ├── START-HERE.md
│   ├── COMPLETE-ALGORITHMS-GUIDE.md
│   ├── by-category/
│   │   ├── 01-sorting.md
│   │   ├── 02-searching.md
│   │   ├── 03-graphs.md
│   │   ├── 04-trees.md
│   │   ├── 05-dynamic-programming.md
│   │   ├── 06-greedy.md
│   │   ├── 07-divide-and-conquer.md
│   │   ├── 08-backtracking.md
│   │   ├── 09-two-pointers.md
│   │   ├── 10-sliding-window.md
│   │   ├── 11-string-algorithms.md
│   │   ├── 12-bit-manipulation.md
│   │   └── 13-math-algorithms.md
│   ├── data-structures/
│   │   ├── arrays-strings.md
│   │   ├── linked-lists.md
│   │   ├── stacks-queues.md
│   │   ├── hash-tables.md
│   │   ├── heaps.md
│   │   ├── trees-tries.md
│   │   └── graphs.md
│   ├── cheatsheets/
│   │   ├── big-o-cheatsheet.md
│   │   ├── pattern-recognition.md
│   │   └── quick-reference.md
│   └── self-test.md
│
├── system-design/                   ← System Design (NEW!)
│   ├── START-HERE.md
│   ├── COMPLETE-SYSTEM-DESIGN-GUIDE.md
│   ├── topics/
│   │   ├── 01-fundamentals.md
│   │   ├── 02-databases.md
│   │   ├── 03-caching.md
│   │   ├── 04-load-balancing.md
│   │   ├── 05-messaging.md
│   │   └── 06-common-systems.md
│   └── cheatsheets/
│       ├── design-template.md
│       └── quick-reference.md
│
├── WEEKLY-PLAN.md                   ← Недельный план (NEW!)
│
└── shared/
    └── resources/
        └── links.md
```

---

## План подготовки

### Behavioral (меньше недели)

| День | Что делать |
|------|------------|
| 1-2 | Прочитай `COMPLETE-ANSWERS-GUIDE.md`. Выучи 6 историй |
| 3-4 | Практикуй вслух. Пройди `self-test.md` |
| 5-6 | Mock interview. Повтори слабые места |
| День X | Утром: `quick-review.md`. Возьми `pocket-cards.md` |

### Algorithms (параллельно)

| День | Что делать |
|------|------------|
| 1-2 | Прочитай `COMPLETE-ALGORITHMS-GUIDE.md`. Выучи Big-O |
| 3-4 | Углубись в слабые категории |
| 5+ | Практикуй задачи, пройди `self-test.md` |

---

## 3 главных правила для интервью

1. **"Я", не "Мы"** — Google хочет понять твой вклад
2. **Цифры** — "улучшилось на 35%", не "стало лучше"
3. **Learning** — каждая история заканчивается уроком

---

## Критерии готовности

### Behavioral
- [ ] Знаю 6 историй наизусть (one-liner + STAR + цифры)
- [ ] Могу рассказать каждую за 2-3 минуты
- [ ] Пройден self-test на 40+/50
- [ ] Проведён хотя бы 1 mock interview

### Algorithms
- [ ] Знаю Big-O для основных алгоритмов
- [ ] Могу выбрать правильный подход по типу задачи
- [ ] Пройден self-test на 40+/50
- [ ] Решено 50+ задач на LeetCode

### System Design
- [ ] Знаю ACED фреймворк для design interview
- [ ] Могу спроектировать 3+ типовые системы
- [ ] Понимаю CAP теорему и trade-offs
- [ ] Знаю когда использовать SQL vs NoSQL, кэширование, очереди

---

## Начни сейчас

### Рекомендуемый порядок

1. **Сначала:** Прочитай [`WEEKLY-PLAN.md`](WEEKLY-PLAN.md) — структурированный план
2. **Behavioral:** [`behavioral/START-HERE.md`](behavioral/START-HERE.md)
3. **Algorithms:** [`algorithms/START-HERE.md`](algorithms/START-HERE.md)
4. **System Design:** [`system-design/START-HERE.md`](system-design/START-HERE.md)

### Если мало времени (3 дня)

| День | Фокус | Файлы |
|------|-------|-------|
| 1 | Behavioral | `COMPLETE-ANSWERS-GUIDE.md`, `stories-compressed.md` |
| 2 | Algorithms | `COMPLETE-ALGORITHMS-GUIDE.md`, `big-o-cheatsheet.md` |
| 3 | System Design + Review | `COMPLETE-SYSTEM-DESIGN-GUIDE.md`, self-tests |

### Если очень мало времени (1 день)

1. `behavioral/cheatsheets/master-cheatsheet.md`
2. `algorithms/cheatsheets/quick-reference.md`
3. `system-design/cheatsheets/quick-reference.md`
4. `behavioral/interview-day/quick-review.md`

---

**Удачи!**
