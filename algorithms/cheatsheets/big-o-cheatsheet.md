# Big-O Cheatsheet

## Иерархия сложностей

```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)
```

### Интуиция для n = 1000:

| O() | Примерно операций | Допустимо? |
|-----|-------------------|------------|
| O(1) | 1 | ✓✓✓ |
| O(log n) | 10 | ✓✓✓ |
| O(n) | 1,000 | ✓✓✓ |
| O(n log n) | 10,000 | ✓✓ |
| O(n²) | 1,000,000 | ✓ (edge) |
| O(n³) | 1,000,000,000 | ✗ |
| O(2ⁿ) | астрономическое | ✗ |

**Правило:** ~10⁸ операций в секунду.

---

## Сортировки

| Алгоритм | Best | Average | Worst | Space |
|----------|------|---------|-------|-------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) |
| **Merge Sort** | O(n log n) | O(n log n) | O(n log n) | O(n) |
| **Quick Sort** | O(n log n) | O(n log n) | O(n²) | O(log n) |
| **Heap Sort** | O(n log n) | O(n log n) | O(n log n) | O(1) |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) |
| Radix Sort | O(d·n) | O(d·n) | O(d·n) | O(n+k) |

**Запомни:** O(n log n) — лучшее для сортировок сравнением.

---

## Структуры данных

### Array

| Операция | Time |
|----------|------|
| Access | O(1) |
| Search | O(n) |
| Insert at end | O(1)* |
| Insert at index | O(n) |
| Delete at end | O(1) |
| Delete at index | O(n) |

*amortized для dynamic array

### Linked List

| Операция | Singly | Doubly |
|----------|--------|--------|
| Access | O(n) | O(n) |
| Search | O(n) | O(n) |
| Insert at head | O(1) | O(1) |
| Insert at tail | O(n)* | O(1) |
| Delete at head | O(1) | O(1) |
| Delete at tail | O(n) | O(1) |

*O(1) если есть tail pointer

### Stack / Queue

| Операция | Time |
|----------|------|
| Push/Enqueue | O(1) |
| Pop/Dequeue | O(1) |
| Peek | O(1) |
| Search | O(n) |

### Hash Table

| Операция | Average | Worst |
|----------|---------|-------|
| Insert | O(1) | O(n) |
| Search | O(1) | O(n) |
| Delete | O(1) | O(n) |

Worst case при плохой hash функции (все коллизии).

### BST (Binary Search Tree)

| Операция | Average | Worst |
|----------|---------|-------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Min/Max | O(log n) | O(n) |

Worst case — вырожденное дерево.

### Balanced BST (AVL, Red-Black)

| Операция | Time |
|----------|------|
| Search | O(log n) |
| Insert | O(log n) |
| Delete | O(log n) |

**Гарантированный** O(log n).

### Heap

| Операция | Time |
|----------|------|
| Insert | O(log n) |
| Extract Min/Max | O(log n) |
| Peek | O(1) |
| Build Heap | O(n) |

### Trie

| Операция | Time |
|----------|------|
| Insert | O(m) |
| Search | O(m) |
| Prefix Search | O(m) |

m = длина слова.

---

## Алгоритмы на графах

| Алгоритм | Time | Space |
|----------|------|-------|
| BFS | O(V + E) | O(V) |
| DFS | O(V + E) | O(V) |
| Dijkstra (heap) | O((V+E) log V) | O(V) |
| Bellman-Ford | O(V · E) | O(V) |
| Floyd-Warshall | O(V³) | O(V²) |
| Topological Sort | O(V + E) | O(V) |
| Union-Find | O(α(n)) ≈ O(1) | O(V) |
| Kruskal | O(E log E) | O(V) |
| Prim | O(E log V) | O(V) |

---

## Поиск

| Алгоритм | Time | Требования |
|----------|------|------------|
| Linear Search | O(n) | Нет |
| Binary Search | O(log n) | Отсортирован |
| Jump Search | O(√n) | Отсортирован |
| Interpolation | O(log log n)* | Равномерное распределение |

*average case

---

## String Matching

| Алгоритм | Time |
|----------|------|
| Naive | O(n·m) |
| KMP | O(n + m) |
| Rabin-Karp | O(n + m)* |
| Z-Algorithm | O(n + m) |

*average case

---

## Dynamic Programming

| Задача | Time | Space |
|--------|------|-------|
| Fibonacci (memo) | O(n) | O(n) |
| Fibonacci (opt) | O(n) | O(1) |
| 0/1 Knapsack | O(n·W) | O(n·W) |
| LCS | O(n·m) | O(n·m) |
| LIS | O(n²) или O(n log n) | O(n) |
| Edit Distance | O(n·m) | O(n·m) |

---

## Backtracking

| Задача | Time |
|--------|------|
| Subsets | O(2ⁿ) |
| Permutations | O(n!) |
| N-Queens | O(n!) |
| Combinations C(n,k) | O(C(n,k)) |

---

## Математика

| Алгоритм | Time |
|----------|------|
| GCD (Euclidean) | O(log(min(a,b))) |
| Sieve of Eratosthenes | O(n log log n) |
| Fast Exponentiation | O(log n) |
| Prime Check (naive) | O(√n) |

---

## Быстрый выбор по сложности

| Допустимо | Выбирай |
|-----------|---------|
| n ≤ 10 | O(n!), O(2ⁿ) |
| n ≤ 20-25 | O(2ⁿ) |
| n ≤ 500 | O(n³) |
| n ≤ 5000 | O(n²) |
| n ≤ 10⁶ | O(n log n) |
| n ≤ 10⁸ | O(n) |
| n > 10⁸ | O(log n), O(1) |

---

## Space Complexity Notes

| Тип | Пример |
|-----|--------|
| O(1) | In-place algorithms |
| O(log n) | Recursion stack (balanced) |
| O(n) | DP table, visited array |
| O(n²) | 2D DP, adjacency matrix |

**Recursion:** Глубина стека = space complexity.

---

## Amortized Analysis

**Amortized O(1)** не значит каждая операция O(1).

Примеры:
- Dynamic Array append: Иногда O(n) при resize, но amortized O(1)
- Union-Find: Каждая операция O(α(n)) ≈ O(1) amortized

---

## Запомни

```
O(1)       — Константа (hash lookup, array access)
O(log n)   — Логарифм (binary search, balanced tree)
O(n)       — Линейный (один проход)
O(n log n) — Оптимальная сортировка
O(n²)      — Квадрат (вложенные циклы)
O(2ⁿ)      — Экспонента (subsets)
O(n!)      — Факториал (permutations)
```
