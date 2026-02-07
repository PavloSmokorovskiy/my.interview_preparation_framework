# Graphs

## Что это

Структура данных из **вершин (vertices)** и **рёбер (edges)**.

```
    1 ——— 2
    |     |
    |     |
    3 ——— 4
```

---

## Терминология

- **Vertex (Node):** Вершина
- **Edge:** Ребро (связь между вершинами)
- **Degree:** Количество рёбер у вершины
- **Path:** Последовательность вершин через рёбра
- **Cycle:** Путь, начинающийся и заканчивающийся в одной вершине
- **Connected:** Есть путь между любыми двумя вершинами
- **Component:** Максимальная связная часть графа

---

## Типы графов

### По направлению

**Undirected (неориентированный):**
```
1 —— 2    (ребро 1-2 = ребро 2-1)
```

**Directed (ориентированный):**
```
1 → 2     (ребро только из 1 в 2)
```

### По весам

**Unweighted:** Все рёбра равнозначны

**Weighted:** Рёбра имеют веса
```
1 —5— 2   (вес ребра = 5)
```

### Специальные типы

**DAG (Directed Acyclic Graph):**
- Ориентированный без циклов
- Topological sort возможен

**Tree:**
- Связный граф без циклов
- n вершин, n-1 рёбер

**Bipartite:**
- Вершины делятся на две группы
- Рёбра только между группами

---

## Представление

### Adjacency List (список смежности) ⭐

```
Граф:  1 — 2
       |   |
       3 — 4

Список:
1 → [2, 3]
2 → [1, 4]
3 → [1, 4]
4 → [2, 3]
```

| Аспект | Complexity |
|--------|------------|
| Space | O(V + E) |
| Check edge | O(degree) |
| All neighbors | O(degree) |
| Add edge | O(1) |

**Когда использовать:** Разреженные графы (E << V²), большинство задач.

### Adjacency Matrix (матрица смежности)

```
    1  2  3  4
1 [ 0, 1, 1, 0 ]
2 [ 1, 0, 0, 1 ]
3 [ 1, 0, 0, 1 ]
4 [ 0, 1, 1, 0 ]
```

| Аспект | Complexity |
|--------|------------|
| Space | O(V²) |
| Check edge | O(1) |
| All neighbors | O(V) |
| Add edge | O(1) |

**Когда использовать:** Плотные графы (E ≈ V²), Floyd-Warshall.

### Edge List

```
[(1,2), (1,3), (2,4), (3,4)]
```

**Когда использовать:** Kruskal's algorithm, простые задачи.

---

## Обход графов

### BFS (Breadth-First Search) ⭐

**Идея:** Волновое распространение от источника.

**Использует:** Queue

**Сложность:** O(V + E)

**Когда использовать:**
- Кратчайший путь (невзвешенный)
- Level-order traversal
- Все вершины на расстоянии K

### DFS (Depth-First Search) ⭐

**Идея:** Идём вглубь до конца, потом backtrack.

**Использует:** Stack (или рекурсия)

**Сложность:** O(V + E)

**Когда использовать:**
- Обход всех вершин
- Cycle detection
- Topological sort
- Connected components
- Path finding (все пути)

### BFS vs DFS

| Аспект | BFS | DFS |
|--------|-----|-----|
| Структура | Queue | Stack |
| Память | O(ширина) | O(глубина) |
| Кратчайший путь | ✓ (невзвешенный) | ✗ |
| Cycle detection | ✓ | ✓ (проще) |

---

## Кратчайший путь

### Single Source

| Алгоритм | Graph Type | Time |
|----------|------------|------|
| BFS | Невзвешенный | O(V + E) |
| Dijkstra | Веса ≥ 0 | O((V+E) log V) |
| Bellman-Ford | Любые веса | O(V · E) |

### All Pairs

| Алгоритм | Time | Когда |
|----------|------|-------|
| Floyd-Warshall | O(V³) | Небольшие графы |
| Dijkstra V раз | O(V(V+E) log V) | Разреженные |

---

## Cycle Detection

### Undirected Graph
- **DFS:** Если встретили visited (не parent) → цикл
- **Union-Find:** Если find(u) == find(v) при добавлении ребра → цикл

### Directed Graph
- **DFS с цветами:**
  - WHITE (не visited)
  - GRAY (в текущем стеке)
  - BLACK (finished)
  - Встретили GRAY → цикл

---

## Topological Sort

**Что:** Линейный порядок вершин DAG, где u → v означает u перед v.

**Алгоритмы:**
1. **DFS:** Добавляем в стек после обработки соседей
2. **Kahn's (BFS):** Начинаем с in-degree = 0

**Применения:**
- Порядок выполнения задач
- Build system dependencies
- Course prerequisites

---

## Union-Find (Disjoint Set)

**Что:** Структура для отслеживания связных компонент.

**Операции:**
- Find(x): К какому множеству принадлежит x?
- Union(x, y): Объединить множества

**Оптимизации:**
- Path compression: При Find сжимаем путь
- Union by rank: Присоединяем меньшее к большему

**Сложность:** O(α(n)) ≈ O(1) амортизированно

**Когда использовать:**
- Динамические компоненты связности
- Kruskal's MST
- Cycle detection (undirected)

---

## MST (Minimum Spanning Tree)

**Что:** Поддерево с минимальным весом, соединяющее все вершины.

### Kruskal's Algorithm
1. Сортируем рёбра по весу
2. Добавляем если не создаёт цикл (Union-Find)

**Time:** O(E log E)

### Prim's Algorithm
1. Начинаем с одной вершины
2. Добавляем минимальное ребро к дереву

**Time:** O(E log V) с priority queue

---

## Типичные задачи

| Задача | Подход |
|--------|--------|
| Number of Islands | DFS/BFS от каждой "1" |
| Clone Graph | BFS/DFS + HashMap для visited |
| Course Schedule | Topological Sort или Cycle Detection |
| Number of Connected Components | DFS/BFS или Union-Find |
| Detect Cycle | DFS с цветами (directed) или Union-Find |
| Shortest Path (unweighted) | BFS |
| Shortest Path (weighted) | Dijkstra |
| Word Ladder | BFS |
| Network Delay Time | Dijkstra |
| Redundant Connection | Union-Find |
| Alien Dictionary | Topological Sort |
| Is Graph Bipartite | BFS/DFS с раскраской |

---

## Bipartite Graph

**Что:** Граф, где вершины можно раскрасить в 2 цвета так, что соседи разных цветов.

**Проверка:** BFS/DFS, чередуя цвета. Если сосед того же цвета → не bipartite.

**Свойство:** Граф bipartite ⟺ нет циклов нечётной длины.

---

## Подводные камни

### 1. Visited set
Всегда отмечай вершины как visited! Иначе — бесконечный цикл.

### 2. Disconnected graph
Не забывай запускать BFS/DFS от всех непосещённых вершин.

### 3. Directed vs Undirected
Cycle detection разный для directed и undirected графов!

### 4. 0-indexed vs 1-indexed
Следи за индексацией вершин.

### 5. Self-loops и multiple edges
Проверяй, допускаются ли в задаче.

---

## Запомни

- **Adjacency List:** O(V+E) space, для большинства задач
- **Adjacency Matrix:** O(V²) space, для плотных графов
- **BFS:** Queue, кратчайший путь (невзвешенный)
- **DFS:** Stack/рекурсия, полный обход, циклы
- **Dijkstra:** Взвешенный граф, веса ≥ 0
- **Topological:** DAG, порядок зависимостей
- **Union-Find:** Связные компоненты, Kruskal
- **Cycle:** Undirected — Union-Find, Directed — DFS с цветами

---

## Deep Theory Layer

### 1) Graph representation определяет производительность

Выбор representation - не деталь, а часть алгоритма.

1. Sparse graph -> adjacency list.
2. Dense graph -> matrix может быть уместнее.
3. Edge-centric задачи -> edge list.

### 2) Directed vs undirected мышление

Многие ошибки происходят из смешения моделей:

- cycle detection,
- degree semantics,
- топологические свойства.

Всегда фиксируй тип графа до выбора метода.

### 3) Weighted semantics

Наличие весов сразу меняет класс корректных алгоритмов:

1. unweighted -> BFS shortest.
2. weighted non-negative -> Dijkstra.
3. negative edges -> Bellman-Ford class.

### 4) Components and connectivity

Связность бывает:

1. connected components (undirected),
2. strongly connected components (directed).

Это разные задачи с разными алгоритмами.

### 5) Контрольные вопросы

1. Почему adjacency matrix может "убить" BFS на sparse graph?
2. Когда edge list удобнее list/matrix?
3. Почему directed cycle detection требует другой логики?
4. Когда выбрать union-find вместо DFS?
5. Что меняется в shortest path при добавлении весов?
