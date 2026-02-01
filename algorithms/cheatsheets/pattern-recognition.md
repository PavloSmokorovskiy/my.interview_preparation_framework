# Pattern Recognition: Как выбрать алгоритм

## Быстрый выбор по ключевым словам

| Ключевое слово в задаче | Вероятный подход |
|------------------------|------------------|
| "Sorted array" | Binary Search |
| "Shortest path" | BFS (невзвеш.) / Dijkstra (взвеш.) |
| "All possibilities" / "permutations" | Backtracking |
| "Maximum/Minimum subarray" | Sliding Window / DP / Kadane |
| "Top K" / "K-th largest" | Heap |
| "Common substring" | DP (LCS) |
| "How many ways" | DP |
| "Can we reach?" | BFS/DFS или DP |
| "Optimal" с подзадачами | DP |
| "Contiguous subarray" | Sliding Window |
| "Two sum" / "pair" | Two Pointers (если sorted) / HashMap |
| "Prefix" | Trie |
| "Connected components" | Union-Find / DFS |
| "Dependencies" / "order" | Topological Sort |
| "Cycle" | DFS (directed) / Union-Find (undirected) |
| "Intervals" / "meetings" | Sorting + Greedy/Heap |

---

## Дерево решений: По типу данных

### Array / List

```
Задача про array?
│
├── Отсортирован?
│   ├── Да → Binary Search
│   └── Нет → Нужна сортировка?
│       ├── Да → Sort + Two Pointers / Binary Search
│       └── Нет → продолжай ниже
│
├── Ищем пару/тройку?
│   ├── Сумма → Two Pointers (sorted) / HashMap
│   └── Другое условие → Two Pointers
│
├── Подмассив (contiguous)?
│   ├── Фиксированный размер → Sliding Window (fixed)
│   ├── Min/max с условием → Sliding Window (variable)
│   └── Оптимальный → DP (Kadane и др.)
│
├── Все подмножества?
│   └── Backtracking O(2ⁿ)
│
└── Преобразование in-place?
    └── Two Pointers (same direction)
```

### String

```
Задача про string?
│
├── Pattern matching?
│   └── KMP / Rabin-Karp
│
├── Palindrome?
│   ├── Check → Two Pointers
│   ├── Longest substring → Expand around center
│   └── Longest subsequence → DP
│
├── Prefix?
│   └── Trie
│
├── Anagram?
│   └── Sorting или Counter
│
├── Подстрока с условием?
│   └── Sliding Window + HashMap
│
└── Edit distance / LCS?
    └── DP 2D
```

### Graph

```
Задача про граф?
│
├── Кратчайший путь?
│   ├── Невзвешенный → BFS
│   ├── Взвешенный (≥0) → Dijkstra
│   └── Отрицательные веса → Bellman-Ford
│
├── Обход всех вершин?
│   └── DFS / BFS
│
├── Cycle detection?
│   ├── Directed → DFS с цветами
│   └── Undirected → Union-Find
│
├── Connected components?
│   └── Union-Find / DFS
│
├── Порядок зависимостей?
│   └── Topological Sort (только DAG)
│
├── MST?
│   └── Kruskal / Prim
│
└── Все пути?
    └── DFS/Backtracking
```

### Tree

```
Задача про дерево?
│
├── Traversal?
│   ├── По уровням → BFS (Level Order)
│   └── In/Pre/Post → DFS
│
├── BST?
│   ├── Поиск → O(log n)
│   ├── Validate → Min/Max bounds
│   └── Kth smallest → Inorder
│
├── Path sum?
│   └── DFS с накоплением
│
├── LCA?
│   ├── BST → Используй свойство
│   └── General → DFS рекурсия
│
└── Serialize?
    └── Preorder + null markers
```

---

## По типу задачи

### Оптимизация (min/max)

```
Нужен оптимум?
│
├── Локальный оптимум = глобальный?
│   └── Да → Greedy
│   └── Нет → продолжай
│
├── Есть подзадачи?
│   └── Да → DP
│
└── Можно проверить "достижимо ли X"?
    └── Да → Binary Search on Answer
```

### Counting (сколько способов)

```
"Сколько способов"?
│
├── Все перечислить?
│   └── Backtracking
│
└── Только количество?
    └── DP
```

### Decision (можно ли)

```
"Можно ли достичь X"?
│
├── Граф/матрица?
│   └── BFS/DFS
│
└── Подзадачи?
    └── DP (boolean)
```

---

## По input size

| n | Допустимая сложность | Типичные алгоритмы |
|---|----------------------|-------------------|
| ≤ 10 | O(n!) | Brute force, Backtracking |
| ≤ 20 | O(2ⁿ) | Backtracking, Bitmask DP |
| ≤ 500 | O(n³) | Floyd-Warshall, некоторые DP |
| ≤ 5000 | O(n²) | Simple DP, nested loops |
| ≤ 10⁶ | O(n log n) | Sorting, Heap |
| ≤ 10⁸ | O(n) | Linear scan, Two Pointers |
| > 10⁸ | O(log n) / O(1) | Binary Search, Math |

---

## Типичные комбинации

### Two Pointers + Sorting
- 3Sum, 4Sum
- Container with Most Water

### HashMap + Linear Scan
- Two Sum (unsorted)
- Longest Substring Without Repeating
- Group Anagrams

### BFS + State
- Word Ladder (state = word)
- Open the Lock (state = combination)
- Rotting Oranges (multi-source BFS)

### DFS + Backtracking
- Permutations, Combinations
- Word Search in Grid
- N-Queens

### Sliding Window + Counter
- Minimum Window Substring
- Permutation in String
- Find All Anagrams

### DP + Knapsack
- Subset Sum
- Coin Change
- Partition Equal Subset

### Binary Search + Greedy
- Capacity to Ship Packages
- Koko Eating Bananas

### Heap + Greedy
- Merge K Sorted Lists
- Meeting Rooms II
- Task Scheduler

### Union-Find + Sorting
- Accounts Merge
- Redundant Connection
- Kruskal's MST

---

## Чек-лист перед решением

1. **Понял ли я задачу?**
   - Inputs, outputs, constraints
   - Edge cases

2. **Какой тип данных?**
   - Array, String, Graph, Tree?

3. **Какой размер input?**
   - Определяет допустимую сложность

4. **Есть ли structure?**
   - Отсортирован?
   - Граф/дерево?

5. **Что ищем?**
   - Оптимум? → DP / Greedy
   - Все варианты? → Backtracking
   - Количество? → DP
   - Да/Нет? → BFS/DFS или DP

6. **Какие паттерны подходят?**
   - Смотри ключевые слова выше

---

## Запомни

- **Sorted + Search** → Binary Search
- **Shortest path** → BFS/Dijkstra
- **All possibilities** → Backtracking
- **Contiguous subarray** → Sliding Window
- **Optimal with subproblems** → DP
- **Local = Global optimal** → Greedy
- **Pair in sorted** → Two Pointers
- **Fast lookup** → HashMap
- **Min/Max dynamically** → Heap
- **Prefix operations** → Trie
- **Connected components** → Union-Find
- **Dependencies** → Topological Sort
