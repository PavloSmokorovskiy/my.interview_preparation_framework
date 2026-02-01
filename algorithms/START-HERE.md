# START HERE: Алгоритмы за 5 минут

> **Время изучения:** ~5 минут (этот файл), ~3-4 часа (полный раздел Algorithms)

**Цель:** Знать, что существует. Понимать, когда применять. Не услышать про алгоритм впервые на собеседовании.

---

## Что здесь есть

```
algorithms/
│
├── START-HERE.md                    ← Ты здесь
├── COMPLETE-ALGORITHMS-GUIDE.md     ← ГЛАВНЫЙ ФАЙЛ (все алгоритмы)
│
├── by-category/                     ← Детальные гайды (13 категорий)
│   ├── 01-sorting.md               ← Сортировки
│   ├── 02-searching.md             ← Поиск
│   ├── 03-graphs.md                ← Графы (BFS, DFS, Dijkstra...)
│   ├── 04-trees.md                 ← Деревья
│   ├── 05-dynamic-programming.md   ← DP
│   ├── 06-greedy.md                ← Жадные алгоритмы
│   ├── 07-divide-and-conquer.md    ← Разделяй и властвуй
│   ├── 08-backtracking.md          ← Перебор с возвратом
│   ├── 09-two-pointers.md          ← Два указателя
│   ├── 10-sliding-window.md        ← Скользящее окно
│   ├── 11-string-algorithms.md     ← Строковые алгоритмы
│   ├── 12-bit-manipulation.md      ← Битовые операции
│   └── 13-math-algorithms.md       ← Математические алгоритмы
│
├── data-structures/                 ← Структуры данных
│   ├── arrays-strings.md
│   ├── linked-lists.md
│   ├── stacks-queues.md
│   ├── hash-tables.md
│   ├── heaps.md
│   ├── trees-tries.md
│   └── graphs.md
│
├── cheatsheets/
│   ├── big-o-cheatsheet.md         ← Сложности всех алгоритмов
│   ├── pattern-recognition.md      ← Как выбрать алгоритм по условию
│   └── quick-reference.md          ← Краткая справка
│
└── self-test.md                     ← Самопроверка
```

---

## Как изучать

### День 1-2: Обзор
1. **Прочитай `COMPLETE-ALGORITHMS-GUIDE.md`** — это обзор всего
2. **Выучи Big-O:** `cheatsheets/big-o-cheatsheet.md`
3. **Пойми pattern recognition:** `cheatsheets/pattern-recognition.md`

### День 3-4: Углубление
1. **Структуры данных:** `data-structures/` — основа всего
2. **Ключевые категории:**
   - Sorting (знать когда какой)
   - Graphs (BFS/DFS — must know)
   - Dynamic Programming (паттерны)

### День 5+: Практика
1. **Пройди `self-test.md`**
2. **Повтори слабые места** по категориям

---

## Мнемоника сложностей

```
O(1)        — Константа (доступ по индексу)
O(log n)    — Логарифм (binary search, сбалансированные деревья)
O(n)        — Линейный (один проход)
O(n log n)  — Оптимальная сортировка (merge, quick, heap)
O(n²)       — Квадрат (вложенные циклы, простые сортировки)
O(2ⁿ)       — Экспонента (рекурсия без memo, подмножества)
O(n!)       — Факториал (перестановки)
```

**Запомни:** `1 < log n < n < n log n < n² < 2ⁿ < n!`

---

## 5 вопросов для выбора алгоритма

1. **Данные отсортированы?** → Binary Search
2. **Нужен кратчайший путь?** → BFS (невзвешенный), Dijkstra (взвешенный)
3. **Есть оптимальная подструктура?** → Dynamic Programming
4. **Локальный оптимум = глобальный?** → Greedy
5. **Нужны все варианты?** → Backtracking / DFS

---

## Категории алгоритмов — краткий обзор

| Категория | Когда использовать | Примеры |
|-----------|-------------------|---------|
| **Sorting** | Нужно упорядочить | Merge Sort, Quick Sort |
| **Searching** | Найти элемент | Binary Search, BFS/DFS |
| **Graphs** | Связи между объектами | Dijkstra, Topological Sort |
| **Trees** | Иерархические данные | Traversals, BST |
| **DP** | Оптимизация с подзадачами | Knapsack, LCS |
| **Greedy** | Локальный выбор оптимален | Activity Selection |
| **Two Pointers** | Массив, ищем пару | Two Sum (sorted) |
| **Sliding Window** | Подмассив/подстрока | Max sum subarray |
| **Backtracking** | Все комбинации | N-Queens, Subsets |
| **Bit Manipulation** | Работа с битами | XOR, маски, степени 2 |
| **Math** | Числовые задачи | GCD, простые числа, модульная арифметика |

---

## Структуры данных — когда какую

| Структура | O(search) | O(insert) | Когда использовать |
|-----------|-----------|-----------|-------------------|
| Array | O(n) | O(1)* | Индексный доступ |
| Hash Table | O(1) | O(1) | Быстрый lookup по ключу |
| BST | O(log n) | O(log n) | Отсортированные данные |
| Heap | O(n) | O(log n) | Min/Max быстро |
| Stack | O(n) | O(1) | LIFO: undo, скобки |
| Queue | O(n) | O(1) | FIFO: BFS, tasks |

*amortized для динамического массива

---

## Приоритет изучения

### Must Know (80% задач)
1. **Binary Search** — поиск в отсортированном
2. **BFS/DFS** — обход графов и деревьев
3. **Hash Table** — O(1) lookup
4. **Two Pointers** — работа с массивами
5. **Sliding Window** — подмассивы/подстроки
6. **Basic DP** — Fibonacci pattern, grid traversal

### Good to Know
7. Merge Sort / Quick Sort — оптимальная сортировка
8. Dijkstra — кратчайший путь с весами
9. Heap — priority queue
10. Trie — работа с префиксами
11. Union-Find — связные компоненты

### Nice to Know
12. Topological Sort — зависимости
13. Segment Tree — range queries
14. KMP — pattern matching

---

## Типичные ошибки

### 1. Забыть про edge cases
- Пустой массив
- Один элемент
- Все элементы одинаковые
- Отрицательные числа

### 2. Неправильная сложность
- HashMap != TreeMap (O(1) vs O(log n))
- ArrayList.add(0, x) = O(n), не O(1)

### 3. Путаница BFS vs DFS
- **BFS** — кратчайший путь (невзвешенный граф)
- **DFS** — обход всех вершин, поиск цикла

---

## Начни сейчас

1. Открой [`COMPLETE-ALGORITHMS-GUIDE.md`](COMPLETE-ALGORITHMS-GUIDE.md)
2. Прочитай обзор каждой категории
3. Выучи Big-O из [`cheatsheets/big-o-cheatsheet.md`](cheatsheets/big-o-cheatsheet.md)

**Удачи!**
