# Mathematical Algorithms (Математические алгоритмы)

## GCD и LCM

### GCD (Greatest Common Divisor)

**Алгоритм Евклида:**

```java
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

// Рекурсивная версия:
int gcdRecursive(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```

**Сложность:** O(log(min(a, b)))

### LCM (Least Common Multiple)

```java
int lcm(int a, int b) {
    return a / gcd(a, b) * b;  // Порядок важен для избежания overflow
}
```

**Формула:** LCM(a, b) = a × b / GCD(a, b)

---

## Простые числа

### Проверка простоты

```java
boolean isPrime(int n) {
    if (n <= 1) return false;
    if (n <= 3) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;

    for (int i = 5; i * i <= n; i += 6) {
        if (n % i == 0 || n % (i + 2) == 0) {
            return false;
        }
    }
    return true;
}
```

**Сложность:** O(√n)

**Оптимизация:** Проверяем только 6k±1 (все простые > 3 имеют такую форму).

---

### Решето Эратосфена (Sieve of Eratosthenes)

**Задача:** Найти все простые числа до n.

```java
boolean[] sieveOfEratosthenes(int n) {
    boolean[] isPrime = new boolean[n + 1];
    Arrays.fill(isPrime, true);
    isPrime[0] = isPrime[1] = false;

    for (int i = 2; i * i <= n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i) {
                isPrime[j] = false;
            }
        }
    }
    return isPrime;
}

// Получить список простых:
List<Integer> getPrimes(int n) {
    boolean[] isPrime = sieveOfEratosthenes(n);
    List<Integer> primes = new ArrayList<>();
    for (int i = 2; i <= n; i++) {
        if (isPrime[i]) primes.add(i);
    }
    return primes;
}
```

**Сложность:** O(n log log n)

---

### Разложение на простые множители

```java
List<Integer> primeFactors(int n) {
    List<Integer> factors = new ArrayList<>();

    // Делим на 2
    while (n % 2 == 0) {
        factors.add(2);
        n /= 2;
    }

    // Проверяем нечётные
    for (int i = 3; i * i <= n; i += 2) {
        while (n % i == 0) {
            factors.add(i);
            n /= i;
        }
    }

    // Если остался простой > √n
    if (n > 2) {
        factors.add(n);
    }

    return factors;
}
```

---

## Степень числа

### Быстрое возведение в степень (Fast Exponentiation)

```java
long power(long base, long exp, long mod) {
    long result = 1;
    base %= mod;

    while (exp > 0) {
        if ((exp & 1) == 1) {  // exp нечётное
            result = (result * base) % mod;
        }
        exp >>= 1;  // exp /= 2
        base = (base * base) % mod;
    }
    return result;
}

// Без модуля:
double myPow(double x, int n) {
    long exp = n;
    if (exp < 0) {
        x = 1 / x;
        exp = -exp;
    }

    double result = 1.0;
    while (exp > 0) {
        if ((exp & 1) == 1) {
            result *= x;
        }
        x *= x;
        exp >>= 1;
    }
    return result;
}
```

**Сложность:** O(log n)

**Идея:** x^n = (x^(n/2))² если n чётное, x · x^(n-1) если нечётное.

---

## Modular Arithmetic

### Основные свойства
```
(a + b) % m = ((a % m) + (b % m)) % m
(a - b) % m = ((a % m) - (b % m) + m) % m
(a * b) % m = ((a % m) * (b % m)) % m
(a / b) % m = ((a % m) * modInverse(b, m)) % m
```

### Modular Inverse

```java
// Используя Fermat's Little Theorem (только для простого mod)
// a^(-1) ≡ a^(p-2) (mod p)
long modInverse(long a, long mod) {
    return power(a, mod - 2, mod);
}
```

---

## Комбинаторика

### Факториал

```java
long factorial(int n) {
    long result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}

// С модулем:
long factorialMod(int n, long mod) {
    long result = 1;
    for (int i = 2; i <= n; i++) {
        result = (result * i) % mod;
    }
    return result;
}
```

### Биномиальные коэффициенты C(n, k)

**Через Pascal's Triangle:**
```java
long binomial(int n, int k) {
    if (k > n - k) k = n - k;  // C(n,k) = C(n, n-k)

    long result = 1;
    for (int i = 0; i < k; i++) {
        result = result * (n - i) / (i + 1);
    }
    return result;
}
```

**Через DP (Pascal's Triangle):**
```java
long[][] pascalTriangle(int n) {
    long[][] C = new long[n + 1][n + 1];
    for (int i = 0; i <= n; i++) {
        C[i][0] = 1;
        for (int j = 1; j <= i; j++) {
            C[i][j] = C[i-1][j-1] + C[i-1][j];
        }
    }
    return C;
}
```

---

## Числа Фибоначчи

### Итеративно O(n)
```java
int fibonacci(int n) {
    if (n <= 1) return n;
    int prev2 = 0, prev1 = 1;
    for (int i = 2; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

### Matrix Exponentiation O(log n)
```java
// F(n) = [[1,1],[1,0]]^n * [F(1), F(0)]
int fibonacciMatrix(int n) {
    if (n <= 1) return n;

    int[][] M = {{1, 1}, {1, 0}};
    int[][] result = matrixPower(M, n - 1);
    return result[0][0];
}

int[][] matrixPower(int[][] M, int n) {
    int[][] result = {{1, 0}, {0, 1}};  // Identity

    while (n > 0) {
        if ((n & 1) == 1) {
            result = matrixMultiply(result, M);
        }
        M = matrixMultiply(M, M);
        n >>= 1;
    }
    return result;
}

int[][] matrixMultiply(int[][] A, int[][] B) {
    return new int[][] {
        {A[0][0]*B[0][0] + A[0][1]*B[1][0], A[0][0]*B[0][1] + A[0][1]*B[1][1]},
        {A[1][0]*B[0][0] + A[1][1]*B[1][0], A[1][0]*B[0][1] + A[1][1]*B[1][1]}
    };
}
```

---

## Геометрия (базовое)

### Расстояние между точками
```java
double distance(int x1, int y1, int x2, int y2) {
    return Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2));
}
```

### Площадь треугольника
```java
double triangleArea(int x1, int y1, int x2, int y2, int x3, int y3) {
    return Math.abs((x1 * (y2 - y3) + x2 * (y3 - y1) + x3 * (y1 - y2)) / 2.0);
}
```

### Проверка коллинеарности
```java
boolean areCollinear(int x1, int y1, int x2, int y2, int x3, int y3) {
    return (y2 - y1) * (x3 - x2) == (y3 - y2) * (x2 - x1);
}
```

---

## Числовые задачи

### Reverse Integer

```java
int reverse(int x) {
    int result = 0;
    while (x != 0) {
        int digit = x % 10;
        x /= 10;

        // Check overflow
        if (result > Integer.MAX_VALUE / 10 ||
            (result == Integer.MAX_VALUE / 10 && digit > 7)) {
            return 0;
        }
        if (result < Integer.MIN_VALUE / 10 ||
            (result == Integer.MIN_VALUE / 10 && digit < -8)) {
            return 0;
        }

        result = result * 10 + digit;
    }
    return result;
}
```

### Palindrome Number

```java
boolean isPalindrome(int x) {
    if (x < 0 || (x % 10 == 0 && x != 0)) {
        return false;
    }

    int reversed = 0;
    while (x > reversed) {
        reversed = reversed * 10 + x % 10;
        x /= 10;
    }

    return x == reversed || x == reversed / 10;
}
```

### Count Digits

```java
int countDigits(int n) {
    if (n == 0) return 1;
    return (int) Math.floor(Math.log10(Math.abs(n))) + 1;
}
```

---

## Sqrt и другие корни

### Integer Square Root (Binary Search)

```java
int mySqrt(int x) {
    if (x < 2) return x;

    long left = 1, right = x / 2;
    while (left <= right) {
        long mid = left + (right - left) / 2;
        if (mid * mid == x) {
            return (int) mid;
        } else if (mid * mid < x) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return (int) right;
}
```

### Newton's Method

```java
int mySqrtNewton(int x) {
    if (x < 2) return x;

    double x0 = x;
    double x1 = (x0 + x / x0) / 2.0;

    while (Math.abs(x0 - x1) >= 1) {
        x0 = x1;
        x1 = (x0 + x / x0) / 2.0;
    }
    return (int) x1;
}
```

---

## Типичные задачи

| Задача | Подход |
|--------|--------|
| GCD / LCM | Алгоритм Евклида |
| Check prime | Проверка до √n |
| All primes to n | Sieve of Eratosthenes |
| Power(x, n) | Fast exponentiation |
| Count primes | Sieve + count |
| Prime factors | Делим на простые |
| Factorial | Итеративно или DP |
| Fibonacci | Итеративно O(n) или Matrix O(log n) |
| Sqrt(x) | Binary search |
| Reverse integer | Mod и деление |
| Palindrome number | Reverse half |

---

## Запомни

- **GCD:** Евклид, O(log(min(a,b)))
- **Prime check:** До √n, оптимизация 6k±1
- **Sieve:** O(n log log n) для всех простых
- **Fast power:** O(log n), x^n = (x^(n/2))²
- **Modular:** (a*b) % m = ((a%m) * (b%m)) % m
- **Fibonacci:** Matrix exponentiation для O(log n)
- **Overflow:** Всегда проверяй при умножении!

---

## Deep Theory Layer

### 1) Number theory mindset

Многие математические interview-задачи - это не формулы, а инварианты делимости/остатков.

Полезный подход:

1. свести к gcd/lcm/mod,
2. понять, какие свойства сохраняются на каждом шаге.

### 2) Euclidean algorithm: почему логарифмический

Переход `gcd(a, b) -> gcd(b, a mod b)` резко уменьшает второй аргумент.

Худший случай связан с соседними числами Фибоначчи,
что и даёт логарифмическую сложность.

### 3) Modular arithmetic: практические правила

1. Складывать/умножать с модулем на каждом шаге.
2. Для деления нужен modular inverse (если существует).
3. Проверять взаимную простоту для inverse по Эйлеру/Ферма.

### 4) Fast exponentiation как бинарная декомпозиция

`n` представляется в двоичном виде.

Каждый бит решает, включаем ли соответствующую степень двойки в ответ.

Это обобщается на:

1. матричное возведение,
2. modular power,
3. binary lifting идеи в графах/деревьях.

### 5) Sieve как precompute-парадигма

Sieve полезен, когда много запросов про простые числа.

Trade-off:

- upfront `O(n log log n)` + память,
- потом быстрые ответы.

### 6) Комбинаторика на интервью

Частая схема:

1. заранее посчитать factorial и inverse factorial,
2. отвечать `nCk mod p` за `O(1)`.

Это критично в задачах с большим числом запросов.

### 7) Numerical stability и overflow

1. Проверяй переполнение до умножения.
2. Для корней/float учитывай epsilon и критерий остановки.
3. Для reverse integer заранее проверяй границы типа.

### 8) Контрольные вопросы

1. Почему Euclid работает корректно?
2. Когда существует modular inverse?
3. Почему `pow` можно считать за `O(log n)`?
4. Когда sieve лучше repeated primality checks?
5. Какие ошибки чаще всего приводят к overflow bugs?
