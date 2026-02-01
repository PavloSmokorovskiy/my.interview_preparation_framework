# Поиск (Searching)

## Обзор

| Алгоритм | Time | Space | Требования |
|----------|------|-------|------------|
| Linear Search | O(n) | O(1) | Нет |
| Binary Search | O(log n) | O(1) | Отсортирован |
| Ternary Search | O(log n) | O(1) | Unimodal функция |
| Jump Search | O(√n) | O(1) | Отсортирован |
| Interpolation Search | O(log log n) | O(1) | Равномерное распределение |

---

## Linear Search

**Идея:** Проверяем каждый элемент по очереди.

**Сложность:** O(n)

**Когда использовать:**
- Данные не отсортированы
- Одноразовый поиск (сортировка дороже)
- Маленький массив

---

## Binary Search ⭐⭐⭐

### Базовый алгоритм

**Идея:** Делим отсортированный массив пополам, сравниваем с серединой, продолжаем в нужной половине.

**Визуализация:**
```
Ищем 23 в [2, 5, 8, 12, 16, 23, 38, 56, 72, 91]

[2, 5, 8, 12, 16, 23, 38, 56, 72, 91]
                 ↑ mid = 16
23 > 16 → ищем справа

                    [23, 38, 56, 72, 91]
                          ↑ mid = 56
23 < 56 → ищем слева

                    [23, 38]
                      ↑ mid = 23
Найдено!
```

**Сложность:**
- Time: O(log n)
- Space: O(1) итеративно

---

### Варианты Binary Search

#### 1. Найти элемент (точное совпадение)
```java
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
return -1;  // не найден
```

#### 2. Lower Bound (первый >= target)
```java
while (left < right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] < target) {
        left = mid + 1;
    } else {
        right = mid;
    }
}
return left;
```
**Применение:** Позиция для вставки, первое вхождение.

#### 3. Upper Bound (первый > target)
```java
while (left < right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] <= target) {
        left = mid + 1;
    } else {
        right = mid;
    }
}
return left;
```
**Применение:** Последнее вхождение = upper_bound - 1.

---

### Binary Search on Answer

**Идея:** Когда ответ монотонен — бинарный поиск по ответу.

**Пример:** Минимальная скорость, чтобы съесть все бананы за H часов.

```
Маленькая скорость → не успеваем (false)
Большая скорость → успеваем (true)

false false false | true true true
                  ↑
                  ищем эту границу
```

**Паттерн:**
```java
int left = minAnswer, right = maxAnswer;
while (left < right) {
    int mid = left + (right - left) / 2;
    if (canAchieve(mid)) {
        right = mid;  // может быть ответ
    } else {
        left = mid + 1;  // точно не ответ
    }
}
return left;
```

---

### Типичные задачи Binary Search

| Задача | Подход |
|--------|--------|
| Search in sorted array | Базовый BS |
| First/last occurrence | Lower/upper bound |
| Search insert position | Lower bound |
| Search in rotated array | Modified BS |
| Find peak element | BS по направлению |
| Sqrt(x) | BS on answer |
| Koko eating bananas | BS on answer |
| Capacity to ship packages | BS on answer |
| Median of two sorted arrays | BS с умным mid |

---

### Search in Rotated Sorted Array

**Идея:** Массив отсортирован, но повёрнут. Одна из половин всегда отсортирована.

```
[4, 5, 6, 7, 0, 1, 2]
         ↑ точка поворота

Проверяем какая половина отсортирована:
- Если arr[left] <= arr[mid] → левая отсортирована
- Иначе → правая отсортирована

Проверяем, может ли target быть в отсортированной половине.
```

---

### Find Peak Element

**Идея:** Элемент больше соседей. Идём в сторону возрастания.

```java
// [1, 2, 3, 1]
//        ↑ peak

if (arr[mid] < arr[mid + 1]) {
    left = mid + 1;  // peak справа
} else {
    right = mid;  // peak слева или здесь
}
```

---

## Ternary Search

**Идея:** Для unimodal функций (один максимум/минимум). Делим на 3 части.

```
      *
     / \
    /   \
   /     \
  *       *
 /         \
left mid1 mid2 right

Если f(mid1) < f(mid2): максимум справа от mid1
Иначе: максимум слева от mid2
```

**Сложность:** O(log n), но константа больше чем у Binary Search.

**Когда использовать:** Функция с одним экстремумом (не битонический массив — там Binary Search проще).

---

## Jump Search

**Идея:** Прыжки по √n, потом линейный поиск.

```
[1, 3, 5, 7, 9, 11, 13, 15, 17, 19]

Прыгаем: 1 → 7 → 13 → 19
Если перепрыгнули → линейный поиск назад
```

**Сложность:** O(√n)

**Когда использовать:** Редко. Binary Search обычно лучше.

---

## Interpolation Search

**Идея:** Угадываем позицию по значению (как в словаре — ищем слово на "К" ближе к началу, чем на "Ц").

```java
int pos = low + ((target - arr[low]) / (arr[high] - arr[low])) * (high - low);
```

**Сложность:**
- Average: O(log log n) при равномерном распределении
- Worst: O(n) при неравномерном

**Когда использовать:** Большие равномерно распределённые данные.

---

## Exponential Search

**Идея:** Сначала находим диапазон экспоненциально, потом Binary Search.

```
Ищем элемент:
1. Проверяем позиции 1, 2, 4, 8, 16, 32...
2. Когда нашли arr[i] > target, binary search в [i/2, i]
```

**Сложность:** O(log n)

**Когда использовать:** Неизвестный размер массива, unbounded arrays.

---

## Подводные камни Binary Search

### 1. Integer Overflow
```java
// Плохо:
int mid = (left + right) / 2;  // overflow при больших left и right

// Хорошо:
int mid = left + (right - left) / 2;
```

### 2. Бесконечный цикл
```
// left < right vs left <= right
// mid = (l+r)/2 vs mid = (l+r+1)/2

Зависит от варианта: lower_bound, upper_bound, exact match
```

### 3. Off-by-one errors
```
// left = mid vs left = mid + 1
// right = mid vs right = mid - 1

Тестируй на маленьких примерах!
```

### 4. Неотсортированный массив
Binary Search работает ТОЛЬКО на отсортированных данных!

---

## Запомни

- **Binary Search:** O(log n), требует отсортированные данные
- **Lower bound:** Первый >= target
- **Upper bound:** Первый > target
- **BS on answer:** Когда можно проверить "можно ли достичь X"
- **Rotated array:** Одна половина всегда отсортирована
- **Peak element:** Идём в сторону возрастания
- **Overflow:** Используй `left + (right - left) / 2`
