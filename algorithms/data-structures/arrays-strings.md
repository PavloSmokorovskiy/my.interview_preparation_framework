# Arrays & Strings

## Array (Массив)

### Что это
Последовательность элементов одного типа, хранящихся в непрерывной области памяти.

### Свойства
- **Размер фиксирован** (в статическом массиве)
- **Индексация с 0**
- **Непрерывная память** → отличная cache locality

### Сложности операций

| Операция | Time | Примечание |
|----------|------|------------|
| Access by index | **O(1)** | Главное преимущество |
| Search | O(n) | Линейный поиск |
| Search (sorted) | O(log n) | Binary search |
| Insert at end | O(1)* | *amortized для dynamic |
| Insert at index | O(n) | Сдвиг элементов |
| Delete at end | O(1) | |
| Delete at index | O(n) | Сдвиг элементов |

### Dynamic Array (ArrayList, Vector)

**Что это:** Массив, который автоматически увеличивается при заполнении.

**Как работает:**
1. Выделяем массив размера N
2. При заполнении выделяем новый размера 2N
3. Копируем элементы
4. Amortized O(1) для добавления в конец

**Doubling strategy:** Каждое i-е добавление может стоить O(i), но суммарно для n операций = O(n) → amortized O(1).

---

## String (Строка)

### Что это
Последовательность символов. В большинстве языков — **immutable**.

### Важные свойства

**Immutability (неизменяемость):**
- Строки нельзя изменить
- "Изменение" создаёт новую строку
- s += "a" в цикле = O(n²)!

**Решение:** StringBuilder / StringBuffer

### Сложности операций

| Операция | Time | Примечание |
|----------|------|------------|
| Access char | O(1) | |
| Length | O(1) | Обычно хранится |
| Concatenate | O(n+m) | Создаёт новую строку |
| Substring | O(k)* | *или O(1) с sharing |
| Compare | O(min(n,m)) | |
| Search | O(n·m) | Naive pattern matching |

---

## Типичные паттерны

### 1. Two Pointers
```
Opposite directions: palindrome check
Same direction: remove duplicates
```

### 2. Sliding Window
```
Подстрока/подмассив с условием
```

### 3. Prefix Sum
```
prefix[i] = sum(arr[0..i])
sum(arr[l..r]) = prefix[r] - prefix[l-1]
```
Построение O(n), запрос O(1).

### 4. In-place reversal
```
Reverse array или часть массива
Rotate array = reverse(all) + reverse(0..k) + reverse(k..n)
```

---

## Типичные задачи

| Задача | Подход |
|--------|--------|
| Two Sum | HashMap или Two Pointers (если sorted) |
| 3Sum | Sort + Two Pointers |
| Container with most water | Two Pointers |
| Trapping rain water | Two Pointers или DP |
| Maximum subarray | Kadane's algorithm |
| Merge intervals | Sort + linear scan |
| Valid palindrome | Two Pointers |
| Longest substring without repeating | Sliding Window |
| Anagram check | Sorting или Counting |

---

## Подводные камни

### 1. String concatenation в цикле
```
# Плохо: O(n²)
result = ""
for char in chars:
    result += char

# Хорошо: O(n)
result = "".join(chars)
```

### 2. Substring создаёт копию
В некоторых языках substring создаёт копию → O(k).

### 3. Array slicing
`arr[1:n]` в Python создаёт копию → O(n).

### 4. Модификация во время итерации
Не модифицируй массив во время прохода по нему!

---

## Запомни

- **Array:** O(1) access, O(n) insert/delete (кроме конца)
- **String:** immutable, конкатенация = O(n)
- **Dynamic Array:** amortized O(1) append
- **Паттерны:** Two Pointers, Sliding Window, Prefix Sum

---

## Deep Theory Layer

### 1) Array как модель памяти

Array даёт `O(1)` доступ по индексу, потому что элементы лежат непрерывно,
и адрес вычисляется арифметически (`base + i * element_size`).

Следствие:

- быстрый random access,
- дорогое вставление в середину из-за сдвига хвоста.

### 2) Dynamic array: амортизация

Удвоение capacity даёт амортизированное `O(1)` append.

Интуиция:

- редкие дорогие копирования "размазываются" по множеству дешёвых append.

### 3) Strings и неизменяемость

Во многих языках string immutable.

Практические последствия:

1. Конкатенация в цикле может быть квадратичной.
2. Нужно использовать builder/buffer-подход.

### 4) Prefix structures

Prefix sums/prefix hashes превращают:

- повторные range queries из `O(n)` в `O(1)` после `O(n)` preprocessing.

Это базовый паттерн "время за память".

### 5) Контрольные вопросы

1. Почему динамический массив не делает insert middle быстрым?
2. Когда append может быть `O(n)` и почему это не противоречит амортизации?
3. Почему string builder важен для performance?
4. Когда prefix sums не применимы?
5. Чем contiguous memory полезна для CPU cache?
