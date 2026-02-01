# Quick Reference: Краткая справка

## Структуры данных — когда какую

| Нужно | Структура | Почему |
|-------|-----------|--------|
| Быстрый lookup по ключу | HashMap | O(1) |
| Отсортированные данные | BST / TreeMap | O(log n) + порядок |
| Min/Max быстро | Heap | O(1) peek, O(log n) extract |
| LIFO (последний первым) | Stack | O(1) push/pop |
| FIFO (первый первым) | Queue | O(1) enqueue/dequeue |
| Prefix search | Trie | O(m) для слова длины m |
| Range queries + updates | Segment Tree | O(log n) |
| Связные компоненты | Union-Find | O(α(n)) ≈ O(1) |

---

## Алгоритмы — быстрый выбор

| Задача | Алгоритм |
|--------|----------|
| Поиск в отсортированном | Binary Search |
| Кратчайший путь (невзвеш.) | BFS |
| Кратчайший путь (взвеш. ≥0) | Dijkstra |
| Кратчайший путь (отриц.) | Bellman-Ford |
| Все пары кратчайших | Floyd-Warshall |
| Топологическая сортировка | DFS/Kahn |
| MST | Kruskal/Prim |
| Цикл в undirected | Union-Find |
| Цикл в directed | DFS с цветами |

---

## Паттерны — когда применять

| Паттерн | Ключевые признаки |
|---------|-------------------|
| Two Pointers | Sorted array, пары, in-place |
| Sliding Window | Contiguous subarray/substring |
| Binary Search | Sorted, "найти min/max с условием" |
| BFS | Shortest path, level order |
| DFS | Полный обход, backtracking |
| DP | Optimal substructure + overlapping |
| Greedy | Local optimal = global |
| Backtracking | Все комбинации/перестановки |
| Heap | Top K, merge K, median |
| Union-Find | Connected components |

---

## Сложности — запомни

```
O(1)       — Hash lookup, array access
O(log n)   — Binary Search, balanced tree
O(n)       — Один проход
O(n log n) — Сортировка (merge/quick/heap)
O(n²)      — Вложенные циклы
O(2ⁿ)      — Подмножества
O(n!)      — Перестановки
```

---

## Binary Search — template

```java
// Найти элемент
int left = 0, right = arr.length - 1;
while (left <= right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] == target) {
        return mid;
    } else if (arr[mid] < target) {
        left = mid + 1;
    } else {
        right = mid - 1;
    }
}
return -1;

// Lower bound (первый >= target)
while (left < right) {
    int mid = (left + right) / 2;
    if (arr[mid] < target) {
        left = mid + 1;
    } else {
        right = mid;
    }
}
return left;
```

---

## BFS — template

```java
Queue<Node> queue = new LinkedList<>();
Set<Node> visited = new HashSet<>();

queue.offer(start);
visited.add(start);

while (!queue.isEmpty()) {
    Node node = queue.poll();
    for (Node neighbor : getNeighbors(node)) {
        if (!visited.contains(neighbor)) {
            visited.add(neighbor);
            queue.offer(neighbor);
        }
    }
}
```

---

## DFS — template

```java
// Рекурсивный
void dfs(Node node, Set<Node> visited) {
    visited.add(node);
    for (Node neighbor : getNeighbors(node)) {
        if (!visited.contains(neighbor)) {
            dfs(neighbor, visited);
        }
    }
}

// Итеративный
Stack<Node> stack = new Stack<>();
stack.push(start);
while (!stack.isEmpty()) {
    Node node = stack.pop();
    if (!visited.contains(node)) {
        visited.add(node);
        for (Node neighbor : getNeighbors(node)) {
            stack.push(neighbor);
        }
    }
}
```

---

## Sliding Window — template

```java
// Variable size
int left = 0;
for (int right = 0; right < arr.length; right++) {
    // Expand: add arr[right]

    while (conditionBroken) {
        // Shrink: remove arr[left]
        left++;
    }

    // Process window [left, right]
}
```

---

## Backtracking — template

```java
void backtrack(List<Integer> state, List<Integer> choices, List<List<Integer>> result) {
    if (isSolution(state)) {
        result.add(new ArrayList<>(state));
        return;
    }

    for (int choice : choices) {
        if (isValid(choice)) {
            state.add(choice);
            backtrack(state, remainingChoices, result);
            state.remove(state.size() - 1);  // backtrack
        }
    }
}
```

---

## DP — подход

```
1. Определи state: dp[i] означает что?
2. Определи transition: dp[i] = f(dp[...])
3. Определи base case
4. Определи ответ: dp[n]? max(dp)?
5. (Опционально) Оптимизируй память
```

---

## Топ-10 типов задач

1. **Array:** Two Sum, 3Sum, Maximum Subarray
2. **String:** Longest Palindrome, Anagram
3. **Linked List:** Reverse, Cycle, Merge
4. **Tree:** Traversals, LCA, Validate BST
5. **Graph:** BFS/DFS, Shortest Path
6. **DP:** Fibonacci, Knapsack, LCS
7. **Sliding Window:** Longest Substring
8. **Binary Search:** Search in Rotated
9. **Backtracking:** Permutations, N-Queens
10. **Heap:** Kth Largest, Merge K Lists

---

## Edge Cases — не забудь

- Empty input
- Single element
- All same elements
- Negative numbers
- Integer overflow
- Null/None values
- Очень большие числа

---

## Мнемоники

**Googleyness:** A-H-B-D-O-H-I-C
**Leadership:** I-I-M-R-D
**Сложности:** 1 < log < n < n·log < n² < 2ⁿ < n!

**BFS:** Queue = кратчайший
**DFS:** Stack = полный обход

**Two Pointers:** Opposite (пары) / Same (in-place)
**Sliding Window:** Fixed (размер k) / Variable (условие)
