# Divide and Conquer (Разделяй и властвуй)

## Что это? ⭐

**Идея:** Разбиваем задачу на меньшие подзадачи, решаем их независимо, объединяем результаты.

### Три шага

1. **Divide:** Разделить на подзадачи
2. **Conquer:** Решить подзадачи (рекурсивно)
3. **Combine:** Объединить результаты

---

## Классические примеры

### Merge Sort

**Divide:** Делим массив пополам
**Conquer:** Сортируем каждую половину
**Combine:** Сливаем отсортированные половины

```
        [38, 27, 43, 3]
            /       \
     [38, 27]     [43, 3]
       /  \         /  \
    [38] [27]    [43]  [3]
       \  /         \  /
     [27, 38]     [3, 43]
            \       /
        [3, 27, 38, 43]
```

**Сложность:** T(n) = 2T(n/2) + O(n) → O(n log n)

---

### Quick Sort

**Divide:** Выбираем pivot, разделяем на < pivot и > pivot
**Conquer:** Сортируем каждую часть
**Combine:** Уже отсортировано (in-place)

```
[3, 7, 8, 5, 2, 1, 9, 4]  pivot=4
         ↓
[3, 2, 1] [4] [7, 8, 5, 9]
    ↓           ↓
[1, 2, 3] [4] [5, 7, 8, 9]
```

**Сложность:** O(n log n) average, O(n²) worst

---

### Binary Search

**Divide:** Сравниваем с серединой, выбираем половину
**Conquer:** Ищем в выбранной половине
**Combine:** Результат подзадачи = ответ

```
Ищем 7 в [1, 3, 5, 7, 9, 11]
              ↑ mid=5
7 > 5 → [7, 9, 11]
         ↑ mid=9
7 < 9 → [7]
         ↑ найден!
```

**Сложность:** T(n) = T(n/2) + O(1) → O(log n)

---

## Master Theorem

Для рекуррентности T(n) = aT(n/b) + O(n^d):

| Условие | Сложность |
|---------|-----------|
| d > log_b(a) | O(n^d) |
| d = log_b(a) | O(n^d log n) |
| d < log_b(a) | O(n^log_b(a)) |

**Примеры:**
- Merge Sort: T(n) = 2T(n/2) + O(n) → a=2, b=2, d=1 → O(n log n)
- Binary Search: T(n) = T(n/2) + O(1) → a=1, b=2, d=0 → O(log n)

---

## Задачи Divide and Conquer

### Maximum Subarray (Kadane альтернатива)

**Идея:** Максимальный подмассив либо полностью слева, справа, или пересекает середину.

```
def maxSubArray(nums, left, right):
    if left == right:
        return nums[left]

    mid = (left + right) // 2

    leftMax = maxSubArray(nums, left, mid)
    rightMax = maxSubArray(nums, mid+1, right)
    crossMax = maxCrossing(nums, left, mid, right)

    return max(leftMax, rightMax, crossMax)
```

**Сложность:** O(n log n)

---

### Merge K Sorted Lists

**Divide and Conquer подход:**
```
def mergeKLists(lists):
    if not lists: return None
    if len(lists) == 1: return lists[0]

    mid = len(lists) // 2
    left = mergeKLists(lists[:mid])
    right = mergeKLists(lists[mid:])

    return mergeTwoLists(left, right)
```

**Сложность:** O(n log k), где k — количество списков

Альтернатива: Min-heap, тоже O(n log k)

---

### Count Inversions

**Инверсия:** Пара (i, j) где i < j и arr[i] > arr[j]

**Идея:** Модифицированный Merge Sort — считаем инверсии при слиянии.

```
При слиянии [2, 4] и [1, 3]:
- Берём 1: оно меньше 2 и 4 → 2 инверсии
- Берём 2
- Берём 3: меньше 4 → 1 инверсия
- Берём 4

Total inversions в merge = 3
```

**Сложность:** O(n log n)

---

### Closest Pair of Points

**Задача:** Найти две ближайшие точки на плоскости.

**Brute force:** O(n²)

**D&C подход:**
1. Сортируем по x
2. Делим пополам вертикальной линией
3. Рекурсивно находим минимум в каждой половине
4. Проверяем точки в полосе шириной 2d около разделителя

**Сложность:** O(n log n)

---

### Median of Two Sorted Arrays

**Задача:** Медиана двух отсортированных массивов за O(log(m+n))

**Идея:** Binary Search по разбиению.

```
nums1: [x1, x2 | x3, x4]  — разбиваем на left и right
nums2: [y1, y2, y3 | y4]

Ищем такое разбиение, где:
- left parts имеют (m+n)/2 элементов
- max(left) <= min(right)
```

**Сложность:** O(log(min(m, n)))

---

### Pow(x, n) — Fast Exponentiation

**Идея:** x^n = (x^(n/2))² если n чётное

```
def pow(x, n):
    if n == 0: return 1
    if n < 0: return 1 / pow(x, -n)

    half = pow(x, n // 2)
    if n % 2 == 0:
        return half * half
    else:
        return half * half * x
```

**Сложность:** O(log n)

---

### Strassen's Matrix Multiplication

**Обычное умножение:** O(n³)

**Strassen:** O(n^2.81) — использует 7 умножений вместо 8 для подматриц.

На практике overhead велик, используется для очень больших матриц.

---

## D&C vs DP

| Divide and Conquer | Dynamic Programming |
|-------------------|---------------------|
| Подзадачи независимы | Подзадачи перекрываются |
| Нет мемоизации | Мемоизация/табуляция |
| Top-down naturally | Top-down или bottom-up |
| Merge Sort, Quick Sort | Fibonacci, Knapsack |

---

## Типичные задачи

| Задача | Подход |
|--------|--------|
| Merge Sort | Классический D&C |
| Quick Sort | Partition + D&C |
| Binary Search | Выбор половины |
| Maximum Subarray | Left/Right/Cross |
| Merge K Lists | Pairwise merge |
| Count Inversions | Modified Merge Sort |
| Closest Pair | Split + strip check |
| Median of Two Arrays | Binary Search на partition |
| Pow(x, n) | x^n = (x^(n/2))² |
| Search in 2D Matrix | D&C по квадрантам |

---

## Когда использовать D&C

### Признаки
1. Задачу можно разбить на независимые подзадачи
2. Подзадачи структурно похожи на исходную
3. Результаты легко объединить

### Сложность
- Обычно O(n log n) или лучше
- Зависит от размера подзадач и стоимости объединения

---

## Запомни

- **D&C:** Divide → Conquer → Combine
- **Классика:** Merge Sort, Quick Sort, Binary Search
- **Master Theorem:** T(n) = aT(n/b) + O(n^d)
- **Отличие от DP:** Подзадачи независимы, не перекрываются
- **Типичные задачи:** Сортировки, поиск, подсчёт инверсий

---

## Deep Theory Layer

### 1) Form of recurrence

D&C почти всегда сводится к:

- `T(n) = aT(n/b) + f(n)`.

Где:

1. `a` - число подзадач,
2. `n/b` - размер подзадачи,
3. `f(n)` - цена разделения и объединения.

### 2) Master Theorem: практическая версия

Сравниваем `f(n)` с `n^(log_b a)`:

1. `f(n)` меньше -> доминируют листья (много маленьких задач).
2. равны -> баланс, получаем дополнительный `log n`.
3. больше -> доминирует merge/combine.

### 3) Почему Merge Sort O(n log n)

- глубина рекурсии `log n`;
- на каждом уровне суммарная merge-работа `O(n)`;
- итог `O(n log n)`.

### 4) Что делает D&C эффективным

1. Независимость подзадач.
2. Дешёвый merge/combination.
3. Схлопывание сложности через геометрию дерева рекурсии.

Если combine-step тяжёлый, преимущества исчезают.

### 5) D&C vs iterative

D&C чаще выигрывает, когда:

1. Естественно распараллеливается.
2. Есть математическая структура "разделить пополам".

Итеративный подход лучше, когда:

- важна низкая константа и нет рекурсивной пользы.

### 6) Error patterns

1. Неверный base case -> бесконечная рекурсия.
2. Потеря элементов при split/merge.
3. Неправильная оценка combine-cost (часто недооценивают).

### 7) Interview follow-up

1. Можно ли сделать алгоритм in-place?
2. Можно ли распараллелить подзадачи?
3. Есть ли смысл заменить D&C на DP из-за overlapping subproblems?

### 8) Контрольные вопросы

1. Почему Binary Search - тоже D&C?
2. Чем D&C концептуально отличается от DP?
3. Когда recursion depth становится проблемой?
4. Почему QuickSort в худшем `O(n^2)` через recurrence?
5. Когда combine-step доминирует итоговую сложность?
