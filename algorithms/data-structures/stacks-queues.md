# Stacks & Queues

## Stack (Стек)

### Что это
**LIFO** — Last In, First Out. Последний пришёл — первый ушёл.

```
    ┌───┐
    │ 3 │ ← top (push/pop здесь)
    ├───┤
    │ 2 │
    ├───┤
    │ 1 │
    └───┘
```

### Операции

| Операция | Time | Описание |
|----------|------|----------|
| push(x) | O(1) | Добавить на верх |
| pop() | O(1) | Удалить с верха |
| peek/top() | O(1) | Посмотреть верхний |
| isEmpty() | O(1) | Проверка на пустоту |
| size() | O(1) | Размер |

### Реализация
- **Array-based:** Индекс top, resize при необходимости
- **Linked List-based:** Push/pop в head

### Когда использовать
- **Undo/Redo** — последнее действие отменяется первым
- **Вызовы функций** — call stack
- **Парные скобки** — открывающая → push, закрывающая → pop и проверка
- **DFS** — нерекурсивная реализация
- **Expression evaluation** — постфиксные выражения
- **Monotonic stack** — next greater element

---

## Queue (Очередь)

### Что это
**FIFO** — First In, First Out. Первый пришёл — первый ушёл.

```
front                              rear
  ↓                                 ↓
┌───┬───┬───┬───┐
│ 1 │ 2 │ 3 │ 4 │ ← enqueue сюда
└───┴───┴───┴───┘
  ↑
dequeue отсюда
```

### Операции

| Операция | Time | Описание |
|----------|------|----------|
| enqueue(x) | O(1) | Добавить в конец |
| dequeue() | O(1) | Удалить из начала |
| front/peek() | O(1) | Посмотреть первый |
| isEmpty() | O(1) | Проверка на пустоту |
| size() | O(1) | Размер |

### Реализация
- **Circular Array:** Используем модуль для wrap-around
- **Linked List:** Enqueue в tail, dequeue из head

### Когда использовать
- **BFS** — обход по уровням
- **Scheduling** — task queue
- **Буферизация** — producer-consumer
- **Кэширование** — LRU cache (с доп. структурами)

---

## Deque (Double-ended Queue)

### Что это
Очередь с доступом с обоих концов.

### Операции

| Операция | Time |
|----------|------|
| addFirst(x) | O(1) |
| addLast(x) | O(1) |
| removeFirst() | O(1) |
| removeLast() | O(1) |
| peekFirst() | O(1) |
| peekLast() | O(1) |

### Когда использовать
- Sliding window maximum (monotonic deque)
- Реализация stack и queue одновременно
- Palindrome check

---

## Monotonic Stack ⭐

### Что это
Стек, в котором элементы всегда в порядке (возрастающем или убывающем).

### Идея
При push убираем элементы, нарушающие порядок.

### Применения
- **Next Greater Element:** Для каждого элемента найти первый больший справа
- **Next Smaller Element:** Для каждого элемента найти первый меньший справа
- **Largest Rectangle in Histogram**
- **Trapping Rain Water** (один из подходов)

### Пример: Next Greater Element
```
arr = [2, 1, 2, 4, 3]
result = [4, 2, 4, -1, -1]

Используем убывающий стек:
- Проходим справа налево
- Убираем из стека всё что ≤ текущего
- Верх стека = next greater
- Push текущий
```

---

## Monotonic Deque

### Что это
Deque для sliding window min/max.

### Идея
- Храним индексы
- Поддерживаем монотонность при добавлении
- Удаляем устаревшие при сдвиге окна

### Применение
**Sliding Window Maximum:**
- Deque хранит индексы в убывающем порядке значений
- Front = максимум в текущем окне
- O(n) вместо O(n·k)

---

## Priority Queue (Очередь с приоритетом)

### Что это
Очередь, где элементы извлекаются по приоритету, не по порядку добавления.

### Реализация
**Heap** — см. heaps.md

### Операции

| Операция | Time |
|----------|------|
| insert(x) | O(log n) |
| extractMin/Max() | O(log n) |
| peek() | O(1) |

### Когда использовать
- Dijkstra's algorithm
- K-th largest/smallest
- Merge K sorted lists
- Task scheduling by priority

---

## Типичные задачи

### Stack

| Задача | Подход |
|--------|--------|
| Valid parentheses | Push открывающие, pop и match закрывающие |
| Min stack | Два стека: основной + минимумов |
| Evaluate postfix | Операнды push, операторы pop два и push результат |
| Next greater element | Monotonic stack |
| Largest rectangle histogram | Monotonic stack |
| Daily temperatures | Monotonic stack |

### Queue

| Задача | Подход |
|--------|--------|
| BFS | Основная структура |
| Level order traversal | Queue + level markers |
| Sliding window maximum | Monotonic deque |
| Implement queue with stacks | Два стека |
| Implement stack with queues | Две очереди |

---

## Реализация Stack через Queue и наоборот

### Stack using 2 Queues
- Push: добавляем в queue, перекладываем все предыдущие
- Pop: просто dequeue

### Queue using 2 Stacks
- Enqueue: push в stack1
- Dequeue: если stack2 пуст, перекладываем из stack1; pop из stack2

---

## Подводные камни

### 1. Pop из пустого
Всегда проверяй isEmpty() перед pop/dequeue!

### 2. Stack overflow
Рекурсивный DFS может переполнить call stack. Используй явный стек.

### 3. Circular queue bounds
Правильно обрабатывай wrap-around с модулем.

---

## Запомни

- **Stack:** LIFO, скобки, DFS, undo, monotonic для next greater/smaller
- **Queue:** FIFO, BFS, scheduling
- **Deque:** Оба конца, sliding window min/max
- **Priority Queue:** По приоритету, heap-based
- **Monotonic:** O(n) для "next greater/smaller" и sliding window задач
