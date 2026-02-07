# Deep Understanding Path: Algorithms for Google Coding Interview

> **Цель:** Прочитать все файлы раздела `algorithms/` и получить не список названий алгоритмов, а глубокое понимание: что решает алгоритм, почему он корректен, какова его сложность и когда он ломается.

> **Фокус:** минимум кода, максимум смысла.

---

## Что значит "понимать алгоритм"

Для каждого алгоритма ты должен уметь объяснить вслух 6 вещей:

1. Какой класс задач он решает.
2. Главная идея (одной фразой).
3. Ключевой инвариант (что остаётся истинным на каждом шаге).
4. Почему оценка сложности именно такая.
5. Когда алгоритм нельзя применять.
6. Какой ближайший альтернативный подход и какой trade-off.

Если не можешь объяснить хотя бы 1 пункт, понимание неполное.

---

## Теоретический минимум (до и во время чтения)

### 1. Асимптотики и модели сложности
- Big-O, Big-Theta, Big-Omega.
- Amortized analysis (dynamic array, union-find).
- Time vs Space trade-off.

Где читать:
- `algorithms/cheatsheets/big-o-cheatsheet.md`
- `algorithms/data-structures/arrays-strings.md`
- `algorithms/data-structures/hash-tables.md`

### 2. Корректность алгоритмов
- Loop invariant.
- Mathematical induction.
- Proof by contradiction.
- Exchange argument (для greedy).

Где это используется:
- Binary Search (инвариант границ).
- Dijkstra/Topological/Union-Find.
- Greedy интервалы, Huffman, Jump Game.

### 3. Рекурсия, деревья рекурсии, рекуррентные соотношения
- Что такое recurrence.
- Master Theorem (в базовом виде).

Где читать:
- `algorithms/by-category/07-divide-and-conquer.md`
- `algorithms/by-category/01-sorting.md`

### 4. Базовая дискретная математика
- Графы: `V`, `E`, path, cycle, DAG, connected components.
- Комбинаторика: `2^n`, `n!`, `C(n,k)`.
- Биты и modular arithmetic.

Где читать:
- `algorithms/data-structures/graphs.md`
- `algorithms/by-category/08-backtracking.md`
- `algorithms/by-category/12-bit-manipulation.md`
- `algorithms/by-category/13-math-algorithms.md`

---

## Полный порядок чтения всех файлов (27/27)

### Фаза 0. Ориентация

1. `algorithms/START-HERE.md`
2. `algorithms/cheatsheets/big-o-cheatsheet.md`

Результат фазы:
- уверенно различаешь `O(log n)`, `O(n)`, `O(n log n)`, `O(n^2)`, `O(2^n)`, `O(n!)`;
- понимаешь, почему на интервью время обычно важнее "красоты" решения.

### Фаза 1. Структуры данных (фундамент)

3. `algorithms/data-structures/arrays-strings.md`
4. `algorithms/data-structures/hash-tables.md`
5. `algorithms/data-structures/linked-lists.md`
6. `algorithms/data-structures/stacks-queues.md`
7. `algorithms/data-structures/heaps.md`
8. `algorithms/data-structures/trees-tries.md`
9. `algorithms/data-structures/graphs.md`

Результат фазы:
- для любой структуры объясняешь, что хранит, какие операции быстрые, и почему;
- умеешь выбирать структуру под задачу до выбора алгоритма.

### Фаза 2. Core patterns (самые частые на Google)

10. `algorithms/by-category/01-sorting.md`
11. `algorithms/by-category/02-searching.md`
12. `algorithms/by-category/09-two-pointers.md`
13. `algorithms/by-category/10-sliding-window.md`
14. `algorithms/by-category/03-graphs.md`
15. `algorithms/by-category/04-trees.md`
16. `algorithms/by-category/05-dynamic-programming.md`
17. `algorithms/by-category/06-greedy.md`
18. `algorithms/by-category/08-backtracking.md`
19. `algorithms/by-category/07-divide-and-conquer.md`

Результат фазы:
- ты распознаёшь паттерн по условию задачи в первые 1-2 минуты;
- умеешь обосновать, почему выбранный подход оптимален или хотя бы достаточен.

### Фаза 3. Advanced/edge topics

20. `algorithms/by-category/11-string-algorithms.md`
21. `algorithms/by-category/12-bit-manipulation.md`
22. `algorithms/by-category/13-math-algorithms.md`

Результат фазы:
- готов к нетипичным вопросам, где важно не brute force, а математическая или битовая идея.

### Фаза 4. Интеграция и проверка понимания

23. `algorithms/cheatsheets/pattern-recognition.md`
24. `algorithms/COMPLETE-ALGORITHMS-GUIDE.md`
25. `algorithms/cheatsheets/quick-reference.md`
26. `algorithms/LEETCODE-PRACTICE.md`
27. `algorithms/self-test.md`

Результат фазы:
- видишь весь ландшафт алгоритмов целиком;
- знаешь, какие темы сильные, а какие требуют добора.

---

## Карта теории: что нужно понимать в каждой категории

| Категория | Суть подхода | Теория, без которой понимание поверхностное | Ключевой инвариант |
|----------|---------------|----------------------------------------------|--------------------|
| Sorting | Упорядочить данные | Нижняя граница `Omega(n log n)` для сравнительных сортировок, стабильность, in-place | После шага часть массива уже в верном относительном порядке |
| Searching | Сузить пространство поиска | Монотонность условия, граничные случаи | Истинный ответ всегда остаётся внутри текущего диапазона |
| Two Pointers | Линейный проход вместо квадрата | Упорядоченность/монотонность движения указателей | Указатели никогда не откатываются назад без причины |
| Sliding Window | Контролируемый contiguous-диапазон | Инвариант окна и условие shrink/expand | Текущее окно всегда валидно или целенаправленно делается валидным |
| Graphs | Работа с отношениями и путями | DAG, cycles, shortest path, connectivity | BFS: слой = расстояние; DFS: стек вызовов описывает текущий путь |
| Trees | Рекурсивная структура данных | Высота/глубина, traversal, BST-свойство | Для BST: left < node < right на всём поддереве |
| DP | Оптимизация через подзадачи | Optimal substructure + overlapping subproblems | `dp[state]` хранит полный ответ для подзадачи |
| Greedy | Локальный выбор с глобальной целью | Exchange argument, cut property | Жадный выбор можно зафиксировать в одном из оптимальных решений |
| Backtracking | Полный перебор с отсечениями | Пространство состояний, pruning, branching factor | Текущее частичное решение всегда консистентно с ограничениями |
| Divide & Conquer | Разбить и объединить | Recurrence, Master theorem | Решение корректно, если корректны подзадачи + merge step |
| String Algorithms | Быстрый поиск/сравнение строк | Префикс-функция, rolling hash, коллизии | KMP не пересматривает символы без необходимости |
| Bit Manipulation | Компактные операции на битах | Двоичное представление, XOR-алгебра | Операция над битами сохраняет нужный шаблон маски |
| Math Algorithms | Математические преобразования | Делимость, modular arithmetic, простые числа | Инварианты делимости/остатков сохраняются на шагах |

---

## Что особенно важно для Google coding interview

### Tier 1 (must know deeply)

- Binary Search + вариации (`lower_bound`, `binary search on answer`)
- BFS/DFS + cycle detection + topological sort
- HashMap/Set patterns
- Two Pointers + Sliding Window
- Tree/BST traversal и базовые tree-проблемы
- Heap/Priority Queue
- DP базовые паттерны (1D/2D, knapsack-like, string DP)

Файлы:
- `algorithms/by-category/02-searching.md`
- `algorithms/by-category/03-graphs.md`
- `algorithms/by-category/04-trees.md`
- `algorithms/by-category/05-dynamic-programming.md`
- `algorithms/by-category/09-two-pointers.md`
- `algorithms/by-category/10-sliding-window.md`
- `algorithms/data-structures/hash-tables.md`
- `algorithms/data-structures/heaps.md`

### Tier 2 (часто дают как усиление)

- Union-Find
- Greedy с доказуемой корректностью
- Backtracking с pruning
- Prefix sum / monotonic structures
- String patterns (KMP/Rabin-Karp)

### Tier 3 (реже, но полезно для сложных задач)

- Segment Tree / Fenwick Tree
- Bitmask tricks / bitmask DP
- Number theory / combinatorics

---

## Формат чтения, который даёт именно понимание

Для каждого файла работай циклами "Понял -> Объяснил -> Проверил":

1. Прочитай раздел и выпиши 1-2 ключевые идеи.
2. Без подглядывания объясни алгоритм голосом за 60-90 секунд.
3. Ответь на 3 вопроса:
- Почему это быстрее naive-подхода?
- Где этот подход ломается?
- Какой конкурентный подход можно выбрать вместо него?
4. Сверься с `algorithms/cheatsheets/pattern-recognition.md`.

---

## Чекпоинты понимания (после чтения всех файлов)

### Чекпоинт A: Структуры данных

Ты должен уметь:
- выбрать структуру под задачу за 30-60 секунд;
- объяснить реальные trade-off между `HashMap`, `Heap`, `BST`, `Trie`, `Union-Find`.

### Чекпоинт B: Алгоритмические паттерны

Ты должен уметь:
- по условию задачи назвать 2-3 правдоподобных подхода;
- аргументированно выбрать один (по времени/памяти/простоте).

### Чекпоинт C: Корректность и сложность

Ты должен уметь:
- объяснить инвариант для Binary Search, BFS, Sliding Window, DP;
- вывести грубую оценку сложности без подсказок.

### Чекпоинт D: Interview readiness

Ты должен уметь:
- решить задачу концептуально даже до кода;
- защищать решение на follow-up: "а что если данные не помещаются в память", "а если поток", "а если онлайн-обновления".

---

## Финальная проверка (Definition of Done)

Раздел `algorithms` считается пройденным глубоко, если:

- прочитаны все 27 файлов из маршрута выше;
- `algorithms/self-test.md` стабильно 40+/50;
- по каждому Tier 1 алгоритму ты можешь дать устное объяснение без кода;
- ты умеешь для новой задачи: распознать паттерн -> обосновать выбор -> назвать сложность -> перечислить edge cases.

---

## Если времени мало, но нужна глубина

1. Пройди весь Tier 1 глубоко.
2. Из Tier 2 закрой минимум Union-Find, Greedy correctness, Backtracking pruning.
3. Используй `algorithms/LEETCODE-PRACTICE.md` только как источник формулировок задач, а не как гонку за количеством.

Главный принцип: лучше 20 задач с полной декомпозицией мышления, чем 100 задач по шаблону без объяснения "почему".
