# Hash Tables

## Что это

Структура данных для хранения пар ключ-значение с (почти) O(1) операциями.

```
Key → hash(key) → index → value

"apple" → hash → 3 → stored at index 3
```

---

## Как работает

### Hash Function
Преобразует ключ в индекс массива.

**Требования:**
- Детерминированность (одинаковый вход = одинаковый выход)
- Равномерное распределение
- Быстрое вычисление

### Collision (коллизия)
Когда разные ключи дают одинаковый индекс.

**Неизбежны:** Birthday paradox — при n ключах и m слотах, коллизии вероятны при n ≈ √m.

---

## Обработка коллизий

### 1. Chaining (цепочки)
Каждый слот — linked list.

```
[0] → null
[1] → (key1, val1) → (key2, val2) → null
[2] → (key3, val3) → null
```

**Плюсы:** Просто, работает при высокой загрузке
**Минусы:** Дополнительная память, плохая cache locality

### 2. Open Addressing (открытая адресация)
При коллизии ищем другой свободный слот.

**Linear Probing:** Следующий слот: (h + 1), (h + 2), ...
- Проблема: clustering (группировка)

**Quadratic Probing:** (h + 1²), (h + 2²), (h + 3²), ...
- Меньше clustering

**Double Hashing:** h1(k) + i·h2(k)
- Лучшее распределение

---

## Сложности

| Операция | Average | Worst |
|----------|---------|-------|
| Insert | O(1) | O(n) |
| Search | O(1) | O(n) |
| Delete | O(1) | O(n) |

**Worst case:** Все ключи в одном слоте (плохая hash функция или атака).

### Load Factor
α = n / m (количество элементов / размер таблицы)

- α < 0.7 — обычно хорошо
- При высоком α — resize (обычно × 2)

---

## Hash Map vs Hash Set

### HashMap (Dictionary)
- Хранит пары (key, value)
- get(key), put(key, value), remove(key)

### HashSet
- Хранит только ключи (уникальные)
- add(key), contains(key), remove(key)

**Реализация:** HashSet = HashMap с dummy values.

---

## Ordered vs Unordered

| Тип | Insert/Search | Ordered | Реализация |
|-----|---------------|---------|------------|
| HashMap | O(1) avg | Нет | Hash table |
| TreeMap | O(log n) | Да | BST (обычно Red-Black) |
| LinkedHashMap | O(1) avg | Порядок вставки | Hash + linked list |

**Когда TreeMap:**
- Нужен порядок ключей
- Нужны range queries (keys in [a, b])

---

## Типичные паттерны

### 1. Counting (подсчёт)
```
Подсчёт частоты элементов
count[element]++
```

### 2. Two Sum Pattern
```
Для каждого x проверяем, есть ли (target - x) в map
```

### 3. Grouping (группировка)
```
Группировка анаграмм:
sorted(word) → list of anagrams
```

### 4. Caching (мемоизация)
```
Кэширование результатов вычислений
if key in cache: return cache[key]
```

### 5. Two-way Mapping
```
Две map: key→value и value→key
Для быстрого поиска в обе стороны
```

---

## Типичные задачи

| Задача | Подход |
|--------|--------|
| Two Sum | HashMap: num → index |
| Group Anagrams | sorted(word) as key |
| Valid Anagram | Counting или sorting |
| Contains Duplicate | HashSet |
| First Unique Character | Counting |
| Longest Substring Without Repeating | HashMap: char → last index |
| Subarray Sum Equals K | Prefix sum + HashMap |
| LRU Cache | HashMap + Doubly Linked List |
| Top K Frequent | HashMap counting + Heap |

---

## Subarray Sum с HashMap

### Идея
Используем prefix sum + HashMap.

### Пример: Subarray Sum Equals K
```
prefix[j] - prefix[i] = k → prefix[j] - k = prefix[i]

Храним count[prefix_sum] = количество раз встречался
Для каждого prefix_sum проверяем count[prefix_sum - k]
```

**Time:** O(n), **Space:** O(n)

---

## Подводные камни

### 1. Mutable keys
Не используй изменяемые объекты как ключи! После изменения hash может поменяться.

### 2. Null handling
Некоторые реализации не поддерживают null keys/values.

### 3. Iteration order
HashMap не гарантирует порядок итерации!

### 4. Hash collision attacks
Можно подобрать ключи с одинаковым hash → O(n) операции.

### 5. Забыть инициализировать
```
# Ошибка: KeyError
count[x] += 1

# Правильно:
count[x] = count.get(x, 0) + 1
# или defaultdict
```

---

## Запомни

- **Hash Table:** O(1) average для insert/search/delete
- **Collision:** Chaining (списки) или Open Addressing (probing)
- **Load Factor:** Держи < 0.7, resize при необходимости
- **HashMap vs TreeMap:** O(1) без порядка vs O(log n) с порядком
- **Паттерны:** Counting, Two Sum, Grouping, Prefix Sum
- **Worst case:** O(n) — плохая hash функция
