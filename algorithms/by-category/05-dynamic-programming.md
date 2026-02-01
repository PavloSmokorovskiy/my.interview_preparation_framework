# Dynamic Programming (Динамическое программирование)

## Что такое DP? ⭐

**Определение:** Метод решения задач путём разбиения на подзадачи с сохранением результатов.

### Два ключевых свойства

1. **Optimal Substructure:** Оптимальное решение содержит оптимальные решения подзадач

2. **Overlapping Subproblems:** Одни подзадачи решаются многократно

**Если оба свойства есть → используй DP.**

---

## Два подхода

### Top-Down (Memoization)
```
Рекурсия + кэш результатов

memo = {}
def solve(state):
    if state in memo:
        return memo[state]
    # base case
    # рекурсивное решение
    memo[state] = result
    return result
```

**Плюсы:** Интуитивно, считает только нужные подзадачи
**Минусы:** Overhead рекурсии, stack overflow

### Bottom-Up (Tabulation)
```
Итеративно заполняем таблицу от простых к сложным

dp[base_case] = value
for state in order:
    dp[state] = f(dp[prev_states])
```

**Плюсы:** Нет рекурсии, можно оптимизировать память
**Минусы:** Нужно продумать порядок вычислений

---

## Как решать DP задачи

### Пошаговый подход

1. **Определи состояние (state)**
   - Что нужно знать, чтобы решить подзадачу?
   - dp[i], dp[i][j], dp[i][j][k]...

2. **Определи переход (transition)**
   - Как связаны состояния?
   - dp[i] = f(dp[i-1], dp[i-2], ...)

3. **Определи base case**
   - Начальные значения

4. **Определи порядок вычислений**
   - От каких состояний зависит текущее?

5. **Оптимизируй память** (опционально)
   - Если dp[i] зависит только от dp[i-1], храни только предыдущее

---

## Классические паттерны

### 1. Fibonacci Pattern

**Форма:** dp[i] зависит от нескольких предыдущих

**Примеры:**
- Fibonacci: dp[i] = dp[i-1] + dp[i-2]
- Climbing Stairs: dp[i] = dp[i-1] + dp[i-2]
- House Robber: dp[i] = max(dp[i-1], dp[i-2] + nums[i])

**Оптимизация памяти:** O(n) → O(1), храним только prev1, prev2

---

### 2. Knapsack Pattern ⭐

**Задача:** Выбрать items с ограничением.

#### 0/1 Knapsack
Каждый item берём или нет.

```
dp[i][w] = max value используя items 0..i-1 с capacity w

dp[i][w] = max(
    dp[i-1][w],                      // не берём item i
    dp[i-1][w-weight[i]] + value[i]  // берём item i
)
```

**Примеры:**
- Subset Sum
- Partition Equal Subset Sum
- Target Sum

#### Unbounded Knapsack
Item можно брать много раз.

```
dp[w] = max(dp[w], dp[w-weight[i]] + value[i])

Порядок: для каждого w, проверяем все items
```

**Примеры:**
- Coin Change (min coins)
- Coin Change II (number of ways)
- Rod Cutting

---

### 3. LCS / LIS Pattern

#### LCS (Longest Common Subsequence)

```
dp[i][j] = длина LCS для s1[0..i-1] и s2[0..j-1]

if s1[i-1] == s2[j-1]:
    dp[i][j] = dp[i-1][j-1] + 1
else:
    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
```

**Примеры:**
- Longest Common Subsequence
- Edit Distance
- Minimum ASCII Delete Sum

#### LIS (Longest Increasing Subsequence)

**O(n²) подход:**
```
dp[i] = длина LIS заканчивающейся на i

dp[i] = max(dp[j] + 1) для всех j < i где arr[j] < arr[i]
```

**O(n log n) подход:** Binary Search + массив "tails"

---

### 4. Grid DP

**Задача:** Путь в матрице (обычно из (0,0) в (n-1,m-1))

```
dp[i][j] = f(dp[i-1][j], dp[i][j-1])
```

**Примеры:**
- Unique Paths: dp[i][j] = dp[i-1][j] + dp[i][j-1]
- Minimum Path Sum: dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
- Dungeon Game: справа налево
- Maximal Square

---

### 5. Interval DP

**Задача:** Оптимум на интервале [i, j]

```
dp[i][j] = f(dp[i][k], dp[k][j]) для всех k в (i, j)
```

**Порядок:** По возрастанию длины интервала

**Примеры:**
- Matrix Chain Multiplication
- Burst Balloons
- Palindrome Partitioning

---

### 6. State Machine DP

**Задача:** Состояния с переходами

**Примеры:**

**Best Time to Buy and Sell Stock:**
```
hold[i] = max(hold[i-1], -prices[i])        // держим акцию
cash[i] = max(cash[i-1], hold[i-1] + prices[i])  // продали
```

**House Robber II (circular):**
```
Два прохода: [0..n-2] и [1..n-1]
```

---

## Признаки DP задачи

| Ключевые слова | Паттерн |
|----------------|---------|
| "Сколько способов...?" | Counting DP |
| "Минимальное/максимальное..." | Optimization DP |
| "Можно ли...?" | Boolean DP |
| "Подпоследовательность" | LCS/LIS |
| "Подмассив/подстрока" | Sliding Window или DP |
| "Путь в матрице" | Grid DP |
| "Разбить на части" | Interval DP |

---

## Типичные задачи по паттернам

### Fibonacci Pattern
| Задача | Transition |
|--------|------------|
| Climbing Stairs | dp[i] = dp[i-1] + dp[i-2] |
| House Robber | dp[i] = max(dp[i-1], dp[i-2] + nums[i]) |
| Decode Ways | dp[i] = dp[i-1] + dp[i-2] (если valid) |
| Tribonacci | dp[i] = dp[i-1] + dp[i-2] + dp[i-3] |

### Knapsack Pattern
| Задача | Тип |
|--------|-----|
| Partition Equal Subset Sum | 0/1 Knapsack |
| Target Sum | 0/1 с +/- |
| Coin Change | Unbounded |
| Coin Change II | Unbounded (counting) |

### String DP
| Задача | Подход |
|--------|--------|
| Longest Common Subsequence | 2D: dp[i][j] |
| Edit Distance | 2D: insert/delete/replace |
| Longest Palindromic Substring | Expand или DP |
| Longest Palindromic Subsequence | LCS(s, reverse(s)) |
| Word Break | dp[i] = any(dp[j] and s[j:i] in dict) |

### Grid DP
| Задача | Подход |
|--------|--------|
| Unique Paths | dp[i][j] = dp[i-1][j] + dp[i][j-1] |
| Minimum Path Sum | + grid[i][j] |
| Maximal Square | min(left, top, diagonal) + 1 |
| Dungeon Game | Справа-налево |

---

## Оптимизация памяти

### 2D → 1D
Если dp[i][j] зависит только от dp[i-1][...]:
```
Вместо dp[n][m] используем dp[m] (одна строка)
Обновляем правильном порядке
```

### 1D → O(1)
Если dp[i] зависит только от dp[i-1], dp[i-2]:
```
prev1, prev2 = base_cases
for i in range(...):
    curr = f(prev1, prev2)
    prev2, prev1 = prev1, curr
```

---

## Подводные камни

### 1. Неправильный порядок вычислений
```
Убедись, что все зависимости уже посчитаны!
```

### 2. Base case
```
Не забудь инициализировать базовые случаи
```

### 3. Индексация
```
dp[i] может означать "первые i элементов" или "элемент i"
Будь консистентен!
```

### 4. Overflow
```
Суммы могут переполняться. Используй MOD если нужно.
```

---

## Шаблон решения

```
1. Понять задачу
2. Найти паттерн (Fibonacci? Knapsack? Grid?)
3. Определить:
   - State: dp[?] означает что?
   - Transition: dp[?] = ?
   - Base case: dp[0] = ?, dp[0][0] = ?
   - Answer: dp[n]? dp[n][m]? max(dp)?
4. Написать top-down или bottom-up
5. Оптимизировать память если нужно
```

---

## Запомни

- **DP = оптимальная подструктура + перекрывающиеся подзадачи**
- **Top-down:** Рекурсия + memo, интуитивнее
- **Bottom-up:** Итерация, можно оптимизировать память
- **Паттерны:** Fibonacci, Knapsack, LCS/LIS, Grid, Interval, State Machine
- **Ключ:** Правильно определить state и transition
- **Memory:** Часто можно оптимизировать 2D → 1D → O(1)
