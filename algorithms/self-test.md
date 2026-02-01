# Самопроверка по алгоритмам

**Цель:** 40+/50 для уверенности на интервью.

---

## Инструкция

1. Отвечай вслух или пиши
2. Засекай время: ~30 секунд на вопрос
3. Проверяй по ответам внизу
4. Отмечай слабые места для повторения

---

## Часть 1: Сложности (10 вопросов)

### Вопросы

1. Сложность Binary Search?
2. Сложность Merge Sort (time/space)?
3. Сложность поиска в HashMap (average/worst)?
4. Сложность BFS на графе?
5. Сложность Dijkstra с priority queue?
6. Сложность построения Heap из массива?
7. Сложность добавления в конец Dynamic Array (amortized)?
8. Сложность операций в Union-Find с оптимизациями?
9. Сложность генерации всех permutations?
10. Какая сортировка гарантирует O(n log n) worst case in-place?

<details>
<summary>Ответы</summary>

1. O(log n)
2. Time: O(n log n), Space: O(n)
3. Average: O(1), Worst: O(n)
4. O(V + E)
5. O((V + E) log V)
6. O(n) — не O(n log n)!
7. O(1) amortized
8. O(α(n)) ≈ O(1) amortized
9. O(n!)
10. Heap Sort

</details>

---

## Часть 2: Структуры данных (10 вопросов)

### Вопросы

11. Когда использовать Heap вместо BST?
12. В чём разница между Stack и Queue?
13. Когда использовать Trie?
14. Как найти цикл в Linked List?
15. Разница между HashMap и TreeMap?
16. Что такое Segment Tree и когда использовать?
17. Как реализован Dynamic Array?
18. Что хранит Union-Find?
19. Когда Adjacency Matrix лучше Adjacency List?
20. Что даёт Monotonic Stack?

<details>
<summary>Ответы</summary>

11. Когда нужен только min/max, не поиск произвольного элемента
12. Stack — LIFO (последний первым), Queue — FIFO (первый первым)
13. Для prefix search, autocomplete, spell checker
14. Fast/Slow pointers (Floyd's cycle detection)
15. HashMap O(1) без порядка, TreeMap O(log n) с порядком
16. Дерево для range queries + point updates, O(log n)
17. Массив с resize ×2 при заполнении, amortized O(1) append
18. Parent array для каждого элемента (какому множеству принадлежит)
19. Для плотных графов (E ≈ V²), когда нужна проверка ребра O(1)
20. Решает "next greater/smaller element" за O(n)

</details>

---

## Часть 3: Алгоритмы на графах (10 вопросов)

### Вопросы

21. Когда BFS, а когда DFS?
22. Какой алгоритм для кратчайшего пути с отрицательными весами?
23. Что такое Topological Sort и когда применим?
24. Как детектировать цикл в directed graph?
25. Разница между Kruskal и Prim?
26. Что делает Dijkstra?
27. Когда использовать Floyd-Warshall?
28. Как проверить, что граф bipartite?
29. Что такое DAG?
30. Какую структуру использует BFS?

<details>
<summary>Ответы</summary>

21. BFS — кратчайший путь (невзвешенный), DFS — полный обход, циклы
22. Bellman-Ford
23. Линейный порядок вершин DAG, где u→v значит u перед v. Для зависимостей
24. DFS с тремя цветами: если встретили GRAY → цикл
25. Kruskal — сортировка рёбер + Union-Find, Prim — grow from vertex + heap
26. Кратчайший путь от источника до всех вершин (веса ≥ 0)
27. Когда нужны расстояния между всеми парами, небольшой граф
28. BFS/DFS с чередованием цветов, если конфликт → не bipartite
29. Directed Acyclic Graph — ориентированный граф без циклов
30. Queue

</details>

---

## Часть 4: Паттерны (10 вопросов)

### Вопросы

31. Когда использовать Two Pointers?
32. Что такое Sliding Window и когда применять?
33. Когда Greedy работает?
34. Два ключевых свойства DP?
35. Что такое Backtracking?
36. Когда использовать Binary Search on Answer?
37. Разница между Memoization и Tabulation?
38. Как распознать задачу на DP?
39. Когда использовать Monotonic Deque?
40. Какой паттерн для "все подмножества"?

<details>
<summary>Ответы</summary>

31. Отсортированный массив + поиск пары, in-place модификация
32. Подмассив/подстрока с условием, contiguous
33. Когда локальный оптимум ведёт к глобальному
34. Optimal substructure + Overlapping subproblems
35. Построение решения по частям с откатом при неудаче
36. Когда ответ монотонен и можно проверить "достижимо ли X"
37. Memo — top-down с рекурсией, Tab — bottom-up итеративно
38. "Сколько способов", "min/max", подзадачи
39. Sliding window min/max за O(n)
40. Backtracking O(2ⁿ)

</details>

---

## Часть 5: Конкретные задачи (10 вопросов)

### Вопросы

41. Как найти K-th largest element эффективно?
42. Как проверить, что BST валидный?
43. Алгоритм для Longest Common Subsequence?
44. Как найти LCA в binary tree?
45. Как решить "Two Sum" за O(n)?
46. Алгоритм для "Longest Palindromic Substring"?
47. Как merge K sorted lists?
48. Как найти медиану в потоке чисел?
49. Алгоритм для "Minimum Window Substring"?
50. Как проверить, что строка — перестановка другой?

<details>
<summary>Ответы</summary>

41. Min-heap размера K или Quick Select O(n)
42. DFS с передачей min/max bounds
43. DP 2D: dp[i][j] = dp[i-1][j-1]+1 если совпадают, иначе max(dp[i-1][j], dp[i][j-1])
44. Рекурсия: если нашли p или q в разных поддеревьях → текущий узел LCA
45. HashMap: для каждого x проверяем есть ли (target - x)
46. Expand around center O(n²) или Manacher O(n)
47. Min-heap с K элементами или Divide and Conquer
48. Two heaps: max-heap для меньшей половины, min-heap для большей
49. Sliding Window + Counter/HashMap
50. Sorting O(n log n) или Counter/HashMap O(n)

</details>

---

## Подсчёт результатов

| Баллы | Уровень | Рекомендация |
|-------|---------|--------------|
| 45-50 | Отлично | Готов к интервью |
| 40-44 | Хорошо | Повтори слабые места |
| 30-39 | Средне | Нужно ещё изучение |
| < 30 | Начинающий | Пройди материал заново |

---

## Слабые места для повторения

| Категория | Ссылка на материал |
|-----------|-------------------|
| Сложности | `cheatsheets/big-o-cheatsheet.md` |
| Структуры данных | `data-structures/` |
| Графы | `by-category/03-graphs.md` |
| Паттерны | `cheatsheets/pattern-recognition.md` |
| DP | `by-category/05-dynamic-programming.md` |

---

## Бонус: Быстрый тест (5 вопросов)

Ответь за 2 минуты:

1. Sorted array + find element = ?
2. Shortest path unweighted = ?
3. All permutations complexity = ?
4. Contiguous subarray with condition = ?
5. Min/max in stream = ?

<details>
<summary>Ответы</summary>

1. Binary Search
2. BFS
3. O(n!)
4. Sliding Window
5. Heap

</details>
