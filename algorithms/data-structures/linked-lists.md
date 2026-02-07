# Linked Lists

## Что это

Последовательность узлов, где каждый узел содержит данные и ссылку на следующий (и/или предыдущий) узел.

```
Singly Linked List:
[data|next] → [data|next] → [data|next] → null

Doubly Linked List:
null ← [prev|data|next] ↔ [prev|data|next] ↔ [prev|data|next] → null
```

---

## Типы

### Singly Linked List
- Каждый узел → следующий
- Можем идти только вперёд
- Меньше памяти

### Doubly Linked List
- Каждый узел ↔ предыдущий и следующий
- Можем идти в обе стороны
- Больше памяти, но проще удаление

### Circular Linked List
- Последний узел → первый
- Нет null в конце

---

## Сложности операций

| Операция | Singly | Doubly |
|----------|--------|--------|
| Access by index | O(n) | O(n) |
| Search | O(n) | O(n) |
| Insert at head | **O(1)** | **O(1)** |
| Insert at tail | O(n)* | **O(1)**\*\* |
| Insert at position | O(n) | O(n) |
| Delete at head | **O(1)** | **O(1)** |
| Delete at tail | O(n) | **O(1)** |
| Delete at position | O(n) | O(n)*** |

\* O(1) если храним tail pointer
\*\* Требуется tail pointer
\*\*\* O(1) если есть ссылка на узел

---

## Array vs Linked List

| Аспект | Array | Linked List |
|--------|-------|-------------|
| Access | O(1) | O(n) |
| Insert/Delete начало | O(n) | O(1) |
| Insert/Delete середина | O(n) | O(1)* |
| Memory | Непрерывная | Фрагментированная |
| Cache | Хорошая locality | Плохая locality |
| Extra memory | Нет | Указатели |

\* Если уже есть ссылка на позицию

**Когда Linked List лучше:**
- Частые вставки/удаления в начало
- Неизвестный размер заранее
- Не нужен random access

---

## Основные паттерны

### 1. Fast & Slow Pointers (Floyd's Cycle Detection)

**Идея:** Два указателя с разной скоростью.

**Применения:**
- **Найти цикл:** Fast движется на 2, slow на 1. Если встретятся → цикл.
- **Найти середину:** Когда fast дойдёт до конца, slow будет в середине.
- **Найти начало цикла:** После встречи, один указатель в начало, оба по 1 → встретятся в начале цикла.

### 2. Dummy Node (Sentinel)

**Идея:** Фиктивный узел перед head упрощает edge cases.

```
dummy → [1] → [2] → [3] → null
```

**Когда использовать:**
- Удаление головы возможно
- Слияние списков
- Любая операция, где head может измениться

### 3. Reversal

**Итеративно:**
```
prev = null, curr = head
while curr:
    next = curr.next
    curr.next = prev
    prev = curr
    curr = next
return prev
```

**Применения:**
- Reverse entire list
- Reverse in groups of K
- Palindrome check (reverse second half)

### 4. Merge Two Lists

**Идея:** Два указателя, выбираем меньший.

**Применения:**
- Merge two sorted lists
- Merge K sorted lists (с heap)

---

## Типичные задачи

| Задача | Подход |
|--------|--------|
| Reverse linked list | Итеративно или рекурсивно |
| Detect cycle | Fast & Slow pointers |
| Find cycle start | Floyd's algorithm |
| Find middle | Fast & Slow |
| Remove Nth from end | Two pointers с gap N |
| Merge two sorted | Two pointers + dummy |
| Check palindrome | Find middle + reverse half + compare |
| Intersection point | Length difference или two pointers |
| Add two numbers | Carry + dummy node |

---

## Подводные камни

### 1. Потеря ссылки
```
# Плохо: потеряли оставшийся список
curr.next = newNode

# Хорошо: сохраняем ссылку
next = curr.next
curr.next = newNode
newNode.next = next
```

### 2. Null pointer
Всегда проверяй на null перед .next!

### 3. Забыть обновить head
При удалении первого элемента нужно обновить head.

### 4. Бесконечный цикл
При модификации проверяй, что не создал цикл.

---

## Запомни

- **Linked List:** O(1) insert/delete, O(n) access
- **Fast/Slow:** цикл, середина, начало цикла
- **Dummy node:** упрощает edge cases
- **Reversal:** prev, curr, next — три указателя
- **Сравнение:** Array лучше для random access, List для частых вставок

---

## Deep Theory Layer

### 1) Linked list как структура ссылок

Сильная сторона:

- дешёвые вставки/удаления при известной позиции узла.

Слабая сторона:

- нет random access,
- плохая cache locality.

### 2) Dummy node как техника упрощения

Sentinel убирает особые случаи для head-операций.

Это снижает число edge-case веток и повышает надёжность кода.

### 3) Fast/slow указатели

Фундаментальная идея:

- разные скорости на одной цепочке выявляют циклы и середину.

Это чистая математика движения, а не эвристика.

### 4) Когда linked list проигрывает array

Для большинства read-heavy задач массивы быстрее из-за locality,
несмотря на теоретические преимущества list по вставкам.

### 5) Контрольные вопросы

1. Почему linked list редко выбирают для interview-задач с частым доступом по индексу?
2. Чем dummy node уменьшает bug surface?
3. Почему reverse list требует аккуратного хранения `next`?
4. Как доказать корректность fast/slow cycle detection?
5. Когда doubly linked list оправдан?
