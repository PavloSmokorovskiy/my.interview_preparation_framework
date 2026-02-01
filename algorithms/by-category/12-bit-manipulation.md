# Bit Manipulation (Битовые операции)

## Основы

### Битовые операторы

| Оператор | Название | Пример | Результат |
|----------|----------|--------|-----------|
| `&` | AND | `5 & 3` | `1` (0101 & 0011 = 0001) |
| `\|` | OR | `5 \| 3` | `7` (0101 \| 0011 = 0111) |
| `^` | XOR | `5 ^ 3` | `6` (0101 ^ 0011 = 0110) |
| `~` | NOT | `~5` | `-6` (инверсия всех битов) |
| `<<` | Left Shift | `5 << 1` | `10` (сдвиг влево = ×2) |
| `>>` | Right Shift | `5 >> 1` | `2` (сдвиг вправо = ÷2) |
| `>>>` | Unsigned Right Shift | `-1 >>> 1` | `2147483647` (Java specific) |

---

## Полезные трюки

### 1. Проверка чётности
```java
// Чётное если последний бит = 0
boolean isEven = (n & 1) == 0;
boolean isOdd = (n & 1) == 1;
```

### 2. Умножение/деление на 2
```java
int doubled = n << 1;      // n * 2
int halved = n >> 1;       // n / 2
int times8 = n << 3;       // n * 8 (2^3)
```

### 3. Установить/сбросить/переключить k-й бит
```java
// Биты нумеруются с 0 справа
int setBit = n | (1 << k);      // Установить k-й бит в 1
int clearBit = n & ~(1 << k);   // Сбросить k-й бит в 0
int toggleBit = n ^ (1 << k);   // Переключить k-й бит
```

### 4. Проверить k-й бит
```java
boolean isSet = (n & (1 << k)) != 0;
```

### 5. Убрать последний установленный бит
```java
int cleared = n & (n - 1);

// Пример: 12 (1100) → 8 (1000)
// 12 & 11 = 1100 & 1011 = 1000
```

**Применение:** Проверка степени двойки, подсчёт битов.

### 6. Получить последний установленный бит
```java
int lastBit = n & (-n);

// Пример: 12 (1100) → 4 (0100)
```

### 7. Проверка степени двойки
```java
boolean isPowerOfTwo = n > 0 && (n & (n - 1)) == 0;

// Степени двойки имеют только один бит = 1
// 8 = 1000, 8 & 7 = 1000 & 0111 = 0000 ✓
```

### 8. Подсчёт установленных битов (popcount)
```java
int countBits(int n) {
    int count = 0;
    while (n != 0) {
        n = n & (n - 1);  // убираем последний бит
        count++;
    }
    return count;
}

// Или встроенный метод:
int count = Integer.bitCount(n);
```

---

## XOR свойства ⭐

```
a ^ a = 0        // XOR с собой = 0
a ^ 0 = a        // XOR с нулём = число
a ^ b = b ^ a    // Коммутативность
(a ^ b) ^ c = a ^ (b ^ c)  // Ассоциативность
```

### Применения XOR

**Найти уникальный элемент:**
```java
// Все элементы парные, один уникальный
int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}
```

**Swap без временной переменной:**
```java
a = a ^ b;
b = a ^ b;  // b = (a ^ b) ^ b = a
a = a ^ b;  // a = (a ^ b) ^ a = b
```

---

## Маски

### Маска из k единиц
```java
int mask = (1 << k) - 1;  // k единиц справа

// k=4: (1 << 4) - 1 = 16 - 1 = 15 = 0...01111
```

### Маска для i-го до j-го бита
```java
int mask = ((1 << (j - i + 1)) - 1) << i;
```

### Извлечь k младших бит
```java
int kBits = n & ((1 << k) - 1);
```

---

## Классические задачи

### Single Number

**Задача:** Найти число, встречающееся один раз (остальные дважды).

```java
int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}
```

**Сложность:** O(n) time, O(1) space

---

### Single Number II

**Задача:** Все числа встречаются трижды, кроме одного.

```java
int singleNumber(int[] nums) {
    int ones = 0, twos = 0;
    for (int num : nums) {
        ones = (ones ^ num) & ~twos;
        twos = (twos ^ num) & ~ones;
    }
    return ones;
}
```

Альтернатива: Подсчёт битов по модулю 3.

---

### Single Number III

**Задача:** Два числа встречаются по одному разу.

```java
int[] singleNumber(int[] nums) {
    int xor = 0;
    for (int num : nums) {
        xor ^= num;
    }

    // Найти любой отличающийся бит
    int diffBit = xor & (-xor);

    int a = 0, b = 0;
    for (int num : nums) {
        if ((num & diffBit) == 0) {
            a ^= num;
        } else {
            b ^= num;
        }
    }
    return new int[]{a, b};
}
```

---

### Counting Bits

**Задача:** Для каждого числа от 0 до n посчитать количество единиц.

```java
int[] countBits(int n) {
    int[] dp = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        dp[i] = dp[i >> 1] + (i & 1);
        // или: dp[i] = dp[i & (i-1)] + 1;
    }
    return dp;
}
```

**Сложность:** O(n)

---

### Reverse Bits

**Задача:** Перевернуть биты 32-битного числа.

```java
int reverseBits(int n) {
    int result = 0;
    for (int i = 0; i < 32; i++) {
        result = (result << 1) | (n & 1);
        n >>= 1;
    }
    return result;
}
```

---

### Missing Number

**Задача:** Найти пропущенное число в [0, n].

```java
int missingNumber(int[] nums) {
    int xor = nums.length;  // включаем n
    for (int i = 0; i < nums.length; i++) {
        xor ^= i ^ nums[i];
    }
    return xor;
}

// Альтернатива: Сумма формула n*(n+1)/2 - actualSum
```

---

### Power of Two

```java
boolean isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}
```

---

### Hamming Distance

**Задача:** Количество позиций, где биты различаются.

```java
int hammingDistance(int x, int y) {
    int xor = x ^ y;
    return Integer.bitCount(xor);

    // Или вручную:
    // int count = 0;
    // while (xor != 0) {
    //     xor = xor & (xor - 1);
    //     count++;
    // }
    // return count;
}
```

---

### Sum of Two Integers (without +/-)

```java
int getSum(int a, int b) {
    while (b != 0) {
        int carry = (a & b) << 1;
        a = a ^ b;
        b = carry;
    }
    return a;
}
```

**Идея:** XOR = сумма без carry, AND << 1 = carry.

---

## Bitmask DP

**Идея:** Использовать битовую маску для представления подмножества.

**Пример:** Travelling Salesman Problem

```java
// dp[mask][i] = минимальная стоимость посетить города в mask, закончив в i
// mask: биты показывают посещённые города

int tsp(int[][] dist) {
    int n = dist.length;
    int[][] dp = new int[1 << n][n];

    // Инициализация
    for (int[] row : dp) Arrays.fill(row, Integer.MAX_VALUE);
    dp[1][0] = 0;  // Начинаем в городе 0

    for (int mask = 1; mask < (1 << n); mask++) {
        for (int last = 0; last < n; last++) {
            if (dp[mask][last] == Integer.MAX_VALUE) continue;
            if ((mask & (1 << last)) == 0) continue;

            for (int next = 0; next < n; next++) {
                if ((mask & (1 << next)) != 0) continue;

                int newMask = mask | (1 << next);
                dp[newMask][next] = Math.min(
                    dp[newMask][next],
                    dp[mask][last] + dist[last][next]
                );
            }
        }
    }

    // Ответ: вернуться в город 0
    int ans = Integer.MAX_VALUE;
    int fullMask = (1 << n) - 1;
    for (int last = 1; last < n; last++) {
        if (dp[fullMask][last] != Integer.MAX_VALUE) {
            ans = Math.min(ans, dp[fullMask][last] + dist[last][0]);
        }
    }
    return ans;
}
```

---

## Типичные задачи

| Задача | Подход |
|--------|--------|
| Single Number | XOR всех элементов |
| Number of 1 Bits | n & (n-1) в цикле или bitCount |
| Power of Two | n & (n-1) == 0 |
| Reverse Bits | Shift и OR |
| Missing Number | XOR или формула суммы |
| Counting Bits | DP: dp[i] = dp[i>>1] + (i&1) |
| Hamming Distance | XOR + bitCount |
| Sum without +/- | XOR + AND << 1 |
| Subset generation | Iterate 0 to 2^n - 1 |

---

## Запомни

- **XOR:** a ^ a = 0, a ^ 0 = a — для поиска уникального
- **n & (n-1):** Убирает последний бит — для подсчёта и степени двойки
- **n & (-n):** Получает последний бит
- **(1 << k) - 1:** Маска из k единиц
- **Bitmask DP:** Когда n ≤ 20 и нужны подмножества
