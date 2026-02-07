# Алгоритмы на графах

## Обзор алгоритмов

| Алгоритм | Задача | Time | Space |
|----------|--------|------|-------|
| BFS | Обход, кратчайший путь (невзв.) | O(V+E) | O(V) |
| DFS | Обход, циклы, топ. сорт. | O(V+E) | O(V) |
| Dijkstra | Кратчайший путь (веса ≥ 0) | O((V+E)log V) | O(V) |
| Bellman-Ford | Кратчайший путь (любые веса) | O(V·E) | O(V) |
| Floyd-Warshall | Все пары кратчайших | O(V³) | O(V²) |
| Topological Sort | Порядок зависимостей | O(V+E) | O(V) |
| Union-Find | Связные компоненты | O(α(n)) | O(V) |
| Kruskal | MST | O(E log E) | O(V) |
| Prim | MST | O(E log V) | O(V) |

---

## BFS (Breadth-First Search) ⭐

### Идея
Обход "волнами" от источника. Сначала все соседи, потом соседи соседей.

### Алгоритм
```
1. Добавляем start в queue
2. Пока queue не пуст:
   a. Извлекаем вершину
   b. Для каждого соседа:
      - Если не visited: добавляем в queue, помечаем visited
```

### Визуализация
```
     1
    / \
   2   3
  / \   \
 4   5   6

BFS от 1: [1] → [2,3] → [4,5,6]
Порядок: 1, 2, 3, 4, 5, 6
```

### Применения
- **Кратчайший путь в невзвешенном графе**
- Level-order traversal
- Проверка связности
- Все вершины на расстоянии K

### Типичные задачи
| Задача | Как BFS помогает |
|--------|-----------------|
| Shortest path (unweighted) | Первое достижение = кратчайший |
| Word ladder | Каждое слово — вершина |
| Rotting oranges | Multi-source BFS |
| 01 Matrix | BFS от всех 0 |
| Open the lock | BFS по состояниям |

---

## DFS (Depth-First Search) ⭐

### Идея
Идём вглубь до упора, потом возвращаемся (backtrack).

### Алгоритм (рекурсивный)
```
def dfs(v):
    visited[v] = true
    for neighbor in adj[v]:
        if not visited[neighbor]:
            dfs(neighbor)
```

### Алгоритм (итеративный)
```
1. Добавляем start в stack
2. Пока stack не пуст:
   a. Извлекаем вершину
   b. Если не visited: помечаем, добавляем соседей в stack
```

### Визуализация
```
     1
    / \
   2   3
  / \   \
 4   5   6

DFS от 1: 1 → 2 → 4 → (back) → 5 → (back) → (back) → 3 → 6
Порядок: 1, 2, 4, 5, 3, 6
```

### Применения
- Обход всех вершин
- **Cycle detection**
- **Topological sort**
- Connected components
- Path finding (все пути)

### DFS с временными метками
```
Для задач вроде cycle detection в directed graph:
- entry_time[v]: когда зашли
- exit_time[v]: когда вышли
- Или три цвета: WHITE, GRAY, BLACK
```

---

## BFS vs DFS

| | BFS | DFS |
|-|-----|-----|
| Структура | Queue | Stack |
| Память | O(ширина) | O(глубина) |
| Кратчайший путь (невзв.) | ✓ | ✗ |
| Топологическая сорт. | ✓ (Kahn) | ✓ |
| Cycle detection | ✓ | ✓ (проще для directed) |

**Правило выбора:**
- Кратчайший путь → BFS
- Полный обход / цикл → DFS
- Дерево → обычно DFS

---

## Dijkstra's Algorithm ⭐

### Идея
Greedy подход: всегда берём вершину с минимальным текущим расстоянием.

### Алгоритм
```
1. dist[start] = 0, остальные = ∞
2. Priority queue с (dist, vertex)
3. Пока queue не пуст:
   a. Извлекаем вершину u с минимальным dist
   b. Для каждого соседа v:
      if dist[u] + weight(u,v) < dist[v]:
          dist[v] = dist[u] + weight(u,v)
          добавляем v в queue
```

### Визуализация
```
        1
    A ───── B
    │       │
  4 │       │ 2
    │       │
    C ───── D
        3

Dijkstra от A:
1. A: 0, B: ∞, C: ∞, D: ∞
2. Берём A: B=1, C=4
3. Берём B: D=1+2=3
4. Берём D: (C через D = 3+3=6 > 4, не улучшаем)
5. Берём C: уже всё обработано

Результат: A=0, B=1, C=4, D=3
```

### Сложность
- С priority queue: O((V+E) log V)
- С простым массивом: O(V²)

### Ограничения
**НЕ работает с отрицательными весами!**

### Типичные задачи
| Задача | Комментарий |
|--------|-------------|
| Network delay time | Стандартный Dijkstra |
| Cheapest flights K stops | Modified с состоянием (vertex, stops) |
| Path with minimum effort | Dijkstra по max edge |

---

## Bellman-Ford Algorithm

### Идея
Релаксация всех рёбер V-1 раз. Работает с отрицательными весами.

### Алгоритм
```
1. dist[start] = 0, остальные = ∞
2. Повторяем V-1 раз:
   Для каждого ребра (u, v, w):
      if dist[u] + w < dist[v]:
          dist[v] = dist[u] + w
3. Проверка на отрицательный цикл:
   Для каждого ребра: если можно улучшить → есть цикл
```

### Сложность
- Time: O(V · E)
- Space: O(V)

### Когда использовать
- Есть отрицательные веса
- Нужно детектировать отрицательный цикл

---

## Floyd-Warshall Algorithm

### Идея
DP: для каждой пары (i,j) проверяем, лучше ли идти через k.

### Алгоритм
```
dist[i][j] = weight(i,j) или ∞

for k = 1 to V:
    for i = 1 to V:
        for j = 1 to V:
            dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
```

### Сложность
- Time: O(V³)
- Space: O(V²)

### Когда использовать
- Нужны расстояния между **всеми парами**
- Небольшой граф (V ≤ 500)

---

## Topological Sort ⭐

### Что это
Линейный порядок вершин DAG, где для каждого ребра u→v вершина u перед v.

### DFS подход
```
1. Запускаем DFS
2. После обработки всех соседей → добавляем в стек
3. Результат = стек в обратном порядке
```

### Kahn's Algorithm (BFS)
```
1. Считаем in-degree каждой вершины
2. Добавляем вершины с in-degree=0 в queue
3. Извлекаем вершину, уменьшаем in-degree соседей
4. Если in-degree стал 0 → добавляем в queue
5. Результат = порядок извлечения
```

### Применения
- Course schedule / prerequisites
- Build order
- Task dependencies

---

## Union-Find (Disjoint Set) ⭐

### Идея
Эффективно отслеживаем "кто в какой группе".

### Операции
- **Find(x):** Найти представителя группы x
- **Union(x, y):** Объединить группы x и y

### Оптимизации
1. **Path compression:** При Find сжимаем путь к корню
2. **Union by rank/size:** Присоединяем меньшее к большему

### Сложность с оптимизациями
O(α(n)) ≈ O(1) амортизированно, где α — обратная функция Аккермана.

### Применения
- Динамические связные компоненты
- Kruskal's MST
- Cycle detection в undirected graph
- "Friend circles", "Accounts merge"

---

## MST: Kruskal's Algorithm

### Идея
Жадно берём рёбра в порядке веса, если не создаёт цикл.

### Алгоритм
```
1. Сортируем рёбра по весу
2. Для каждого ребра (u, v, w):
   if Find(u) != Find(v):
       добавляем ребро в MST
       Union(u, v)
```

### Сложность
O(E log E) — сортировка доминирует

### Когда использовать
Разреженные графы, удобно с Union-Find

---

## MST: Prim's Algorithm

### Идея
Растим дерево от одной вершины, добавляя минимальное ребро к дереву.

### Алгоритм
```
1. Начинаем с произвольной вершины
2. Priority queue с рёбрами из дерева
3. Извлекаем минимальное ребро
4. Если ведёт к новой вершине → добавляем, кладём её рёбра в queue
```

### Сложность
O(E log V) с priority queue

### Когда использовать
Плотные графы

---

## Cycle Detection

### Undirected Graph

**Union-Find:**
```
Для каждого ребра (u, v):
    if Find(u) == Find(v):
        есть цикл!
    else:
        Union(u, v)
```

**DFS:**
```
Если встретили visited вершину, которая не parent → цикл
```

### Directed Graph

**DFS с тремя цветами:**
```
WHITE = не посещена
GRAY = в текущем стеке (обрабатывается)
BLACK = обработана

Если встретили GRAY → back edge → цикл
```

---

## Типичные задачи

| Задача | Алгоритм |
|--------|----------|
| Number of islands | DFS/BFS |
| Clone graph | BFS/DFS + HashMap |
| Course schedule | Topological Sort / Cycle Detection |
| Course schedule II | Topological Sort |
| Number of connected components | DFS / Union-Find |
| Redundant connection | Union-Find |
| Network delay time | Dijkstra |
| Cheapest flights K stops | Modified Dijkstra / BFS |
| Word ladder | BFS |
| Alien dictionary | Topological Sort |
| Is graph bipartite? | BFS/DFS с раскраской |
| Critical connections | Tarjan's (bridges) |

---

## Запомни

- **BFS:** Queue, кратчайший путь (невзвешенный), по уровням
- **DFS:** Stack/рекурсия, полный обход, циклы, топ. сорт.
- **Dijkstra:** Priority Queue, взвешенный граф, веса ≥ 0
- **Bellman-Ford:** Отрицательные веса, O(V·E)
- **Floyd-Warshall:** Все пары, O(V³)
- **Topological:** DAG, DFS + стек или Kahn's BFS
- **Union-Find:** Связные компоненты, почти O(1)
- **MST:** Kruskal (сорт. рёбер) или Prim (grow from vertex)

---

## Deep Theory Layer

### 1) Моделирование: что считать вершиной

Критическая часть graph задач - корректно выбрать state graph.

Примеры state-as-node:

1. `(x, y)` в grid.
2. `(word, steps)` в word ladder вариантах.
3. `(city, stops_used)` в constrained shortest path.
4. `(node, mask)` в bitmask-state graph.

Неверный state почти всегда делает правильный алгоритм бесполезным.

### 2) Инварианты BFS/DFS

#### BFS invariant

Когда вершина впервые извлекается из queue:

- найден кратчайший путь по числу рёбер в невзвешенном графе.

#### DFS invariant

Текущий recursion stack:

- описывает текущий путь от root до active node.
- back edge в directed graph указывает на цикл.

### 3) Dijkstra correctness sketch

Условие: все веса `>= 0`.

Инвариант:

- когда вершина `u` выбрана как минимальная по текущей дистанции, `dist[u]` финален.

Почему:

- любой альтернативный путь через ещё не выбранные вершины не может стать меньше, т.к. добавляет неотрицательные веса к уже не меньшим дистанциям.

### 4) Когда Dijkstra ломается

При отрицательных рёбрах "финальность" нарушается:

- вершина, уже извлечённая как "лучшая", может позже улучшиться.

Тогда нужны:

- Bellman-Ford,
- SPFA variants (осторожно с worst-case),
- Johnson (для all-pairs с reweighting).

### 5) Topological Sort и DAG

Topological ordering существует тогда и только тогда, когда граф ацикличен.

Практический тест через Kahn:

- если обработано меньше `V` вершин -> есть цикл.

### 6) Complexity by representation

Adjacency list vs matrix меняет реальные costs:

1. BFS/DFS на list: `O(V + E)`.
2. BFS/DFS на matrix: `O(V^2)`.
3. Проверка ребра `u->v`: matrix `O(1)`, list `O(deg(u))`.

Умение выбрать представление часто важнее выбора алгоритма.

### 7) MST: cut intuition

Для любого "разреза" графа минимальное ребро, пересекающее разрез, безопасно включать в MST.

Из этого следует корректность greedy-step в Kruskal/Prim.

### 8) Interview follow-up

1. Граф не помещается в память -> external/streaming strategies.
2. Нужны динамические обновления рёбер -> dynamic connectivity structures.
3. Нужны path queries online -> preprocessing (LCA, binary lifting, HLD).

### 9) Контрольные вопросы

1. Почему BFS shortest только для невзвешенного?
2. Почему Kahn обнаруживает цикл?
3. Что меняется в сложностях при matrix representation?
4. Почему union-find подходит для undirected cycle detection, но не для directed?
5. Когда выбирать Bellman-Ford вместо Dijkstra?
