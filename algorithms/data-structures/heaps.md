# Heaps

## Что это

**Complete binary tree** со свойством heap:
- **Min-Heap:** Родитель ≤ детей → минимум в корне
- **Max-Heap:** Родитель ≥ детей → максимум в корне

```
Min-Heap:           Max-Heap:
     1                   9
    / \                 / \
   3   2               7   8
  / \                 / \
 7   4               3   5
```

---

## Свойства

### Complete Binary Tree
- Все уровни заполнены, кроме последнего
- Последний уровень заполняется слева направо
- Высота = O(log n)

### Array Representation
Хранится как массив без указателей:
```
     1
    / \
   3   2
  / \
 7   4

Array: [1, 3, 2, 7, 4]
Index:  0  1  2  3  4

Parent(i) = (i - 1) / 2
Left(i)   = 2i + 1
Right(i)  = 2i + 2
```

---

## Операции

| Операция | Time | Описание |
|----------|------|----------|
| peek() | **O(1)** | Получить min/max |
| insert(x) | O(log n) | Добавить элемент |
| extractMin/Max() | O(log n) | Удалить и вернуть min/max |
| buildHeap() | **O(n)** | Построить из массива |
| heapify(i) | O(log n) | Восстановить свойство |

### Insert (push)
1. Добавляем в конец (последний уровень)
2. **Sift Up:** Меняем с родителем пока не восстановим свойство

### Extract (pop)
1. Берём корень (min/max)
2. Перемещаем последний элемент в корень
3. **Sift Down:** Меняем с меньшим/большим ребёнком пока не восстановим свойство

### Build Heap
**Наивно:** n insertов = O(n log n)

**Оптимально:** Heapify снизу вверх = **O(n)**
```
for i from n/2 down to 0:
    heapify(i)
```

---

## Priority Queue

**Heap** — стандартная реализация Priority Queue.

### Когда использовать Priority Queue
- Нужен быстрый доступ к min/max
- Элементы приходят динамически
- Dijkstra's algorithm
- K-th largest/smallest
- Task scheduling by priority

---

## Типичные паттерны

### 1. K-th Largest Element
**Min-Heap размера K:**
- Держим K наибольших элементов
- Корень = K-th largest
- При добавлении: если > корень, replace и heapify

**Time:** O(n log k)

### 2. K-th Smallest Element
**Max-Heap размера K:** (или min-heap со всеми элементами)

### 3. Top K Frequent
1. Count frequencies (HashMap)
2. Heap по частоте
3. Извлекаем K раз

### 4. Merge K Sorted Lists
1. Min-heap с первыми элементами каждого списка
2. Extract min → добавляем в результат
3. Insert следующий из того же списка
4. Повторяем

**Time:** O(n log k), где n — общее количество элементов

### 5. Find Median in Stream
**Два heap:**
- Max-heap для меньшей половины
- Min-heap для большей половины

```
Median:
- Если размеры равны: (maxHeap.top + minHeap.top) / 2
- Иначе: top большего heap
```

---

## Heap Sort

1. Build max-heap: O(n)
2. Извлекаем max, ставим в конец, heapify: O(n log n)

**Total:** O(n log n), **Space:** O(1) in-place

**Vs Quick Sort:** Гарантированный O(n log n), но хуже cache locality.

---

## Типичные задачи

| Задача | Подход |
|--------|--------|
| Kth Largest Element | Min-heap размера K |
| Top K Frequent Elements | HashMap + min-heap |
| Merge K Sorted Lists | Min-heap |
| Find Median from Stream | Two heaps |
| Sort Nearly Sorted Array | Min-heap размера K |
| Sliding Window Maximum | Можно max-heap, но лучше deque |
| Smallest Range Covering K Lists | Min-heap |
| Task Scheduler | Max-heap + cooldown |

---

## Min-Heap vs Max-Heap

**По умолчанию в большинстве языков:** Min-Heap

**Как сделать Max-Heap:**
- Отрицательные значения: push(-x), pop → -result
- Или custom comparator

---

## Heap vs BST

| Аспект | Heap | BST (balanced) |
|--------|------|----------------|
| Find min/max | O(1) | O(log n)* |
| Insert | O(log n) | O(log n) |
| Delete min/max | O(log n) | O(log n) |
| Search arbitrary | O(n) | O(log n) |
| In-order traversal | O(n log n) | O(n) |

*O(1) если храним указатели на min/max

**Когда Heap лучше:**
- Нужен только min/max
- Проще реализация
- Лучше cache locality (массив)

**Когда BST лучше:**
- Нужен поиск произвольного элемента
- Нужен порядок элементов
- Нужны range queries

---

## Подводные камни

### 1. Heap ≠ отсортированный массив
Heap только гарантирует min/max в корне, не полный порядок!

### 2. Decrease key требует знания позиции
В стандартной реализации нет efficient decrease-key.
Для Dijkstra иногда используют "lazy deletion".

### 3. Build Heap — O(n), не O(n log n)
Heapify снизу вверх = линейное время.

### 4. Не забывай про тип heap
Min-heap для K largest, Max-heap для K smallest (интуитивно наоборот!).

---

## Запомни

- **Heap:** Complete binary tree, min/max в корне, array-based
- **Операции:** O(1) peek, O(log n) insert/extract, O(n) build
- **Priority Queue:** Обычно реализована через heap
- **K-th largest:** Min-heap размера K
- **Median stream:** Two heaps (max-heap + min-heap)
- **Merge K lists:** Min-heap с K элементами

---

## Deep Theory Layer

### 1) Heap invariant

Для min-heap:

- каждый узел <= своих детей.

Это локальный инвариант, который гарантирует глобально:

- минимум в корне.

Но не гарантирует полный порядок между соседними ветками.

### 2) Почему build-heap O(n)

Интуитивно:

- большинство узлов близко к листьям и требуют мало sift-down шагов.

Суммарная работа по уровням даёт линейную асимптотику,
хотя отдельный sift-down может быть `O(log n)`.

### 3) Heap vs BST

Heap:

- лучший для repeated extract-min/max.

BST:

- лучший для ordered operations (`next`, `range`, search by key).

### 4) Top-K стратегии

1. Min-heap размера `k`: `O(n log k)`.
2. Full sort: `O(n log n)`.
3. Quickselect average `O(n)`.

Выбор зависит от k, стабильности, необходимости полного порядка.

### 5) Контрольные вопросы

1. Почему heap не подходит для поиска произвольного элемента за `O(log n)`?
2. Почему build-heap линейный?
3. Когда min-heap лучше max-heap в задаче top-k?
4. Какие риски у lazy deletion в priority queue?
5. Почему heap operations часто быстрее дерева по константам?
