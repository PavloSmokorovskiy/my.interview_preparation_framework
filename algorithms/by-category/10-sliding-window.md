# Sliding Window (Скользящее окно)

## Что это? ⭐

**Идея:** "Окно" (подмассив/подстрока), которое скользит по данным для решения задач за O(n).

**Когда использовать:**
- Задача про **contiguous** подмассив/подстроку
- Нужен min/max/sum окна
- Оптимизация brute force O(n²) → O(n)

---

## Два типа

### 1. Fixed Size Window (фиксированный размер)

**Когда:** Размер окна k задан.

```java
// Инициализация первого окна
WindowState windowState = compute(arr, 0, k);

for (int i = k; i < n; i++) {
    // Добавляем новый элемент
    add(arr[i]);
    // Убираем старый элемент
    remove(arr[i - k]);
    // Обрабатываем результат
    process(windowState);
}
```

### 2. Variable Size Window (переменный размер)

**Когда:** Ищем окно с условием (min/max размер).

```java
int left = 0;
for (int right = 0; right < n; right++) {
    // Расширяем: добавляем arr[right]
    add(arr[right]);

    while (conditionBroken) {
        // Сужаем: убираем arr[left]
        remove(arr[left]);
        left++;
    }

    // Обрабатываем текущее окно [left, right]
    processWindow();
}
```

---

## Классические задачи

### Maximum Sum Subarray of Size K (Fixed)

**Задача:** Максимальная сумма подмассива размера k.

```java
public int maxSumSubarray(int[] arr, int k) {
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    int maxSum = windowSum;

    for (int i = k; i < arr.length; i++) {
        windowSum += arr[i] - arr[i - k];
        maxSum = Math.max(maxSum, windowSum);
    }

    return maxSum;
}
```

**Сложность:** O(n)

---

### Minimum Size Subarray Sum (Variable) ⭐

**Задача:** Минимальная длина подмассива с суммой >= target.

```java
public int minSubArrayLen(int target, int[] nums) {
    int left = 0;
    int currSum = 0;
    int minLen = Integer.MAX_VALUE;

    for (int right = 0; right < nums.length; right++) {
        currSum += nums[right];

        while (currSum >= target) {
            minLen = Math.min(minLen, right - left + 1);
            currSum -= nums[left];
            left++;
        }
    }

    return minLen != Integer.MAX_VALUE ? minLen : 0;
}
```

**Сложность:** O(n) — каждый элемент добавляется и удаляется максимум раз.

---

### Longest Substring Without Repeating Characters ⭐

**Задача:** Длина самой длинной подстроки без повторов.

```java
public int lengthOfLongestSubstring(String s) {
    Set<Character> charSet = new HashSet<>();
    int left = 0;
    int maxLen = 0;

    for (int right = 0; right < s.length(); right++) {
        while (charSet.contains(s.charAt(right))) {
            charSet.remove(s.charAt(left));
            left++;
        }

        charSet.add(s.charAt(right));
        maxLen = Math.max(maxLen, right - left + 1);
    }

    return maxLen;
}
```

**Оптимизация с HashMap:**
```java
public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> charIndex = new HashMap<>();
    int left = 0;
    int maxLen = 0;

    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        if (charIndex.containsKey(c) && charIndex.get(c) >= left) {
            left = charIndex.get(c) + 1;
        }

        charIndex.put(c, right);
        maxLen = Math.max(maxLen, right - left + 1);
    }

    return maxLen;
}
```

---

### Longest Substring with At Most K Distinct Characters

**Задача:** Максимальная подстрока с не более k различными символами.

```java
public int lengthOfLongestSubstringKDistinct(String s, int k) {
    Map<Character, Integer> charCount = new HashMap<>();
    int left = 0;
    int maxLen = 0;

    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        charCount.put(c, charCount.getOrDefault(c, 0) + 1);

        while (charCount.size() > k) {
            char leftChar = s.charAt(left);
            charCount.put(leftChar, charCount.get(leftChar) - 1);
            if (charCount.get(leftChar) == 0) {
                charCount.remove(leftChar);
            }
            left++;
        }

        maxLen = Math.max(maxLen, right - left + 1);
    }

    return maxLen;
}
```

---

### Minimum Window Substring ⭐

**Задача:** Минимальное окно в s, содержащее все символы t.

```java
public String minWindow(String s, String t) {
    Map<Character, Integer> need = new HashMap<>();
    for (char c : t.toCharArray()) {
        need.put(c, need.getOrDefault(c, 0) + 1);
    }

    Map<Character, Integer> have = new HashMap<>();
    int haveCount = 0;
    int needCount = need.size();

    int left = 0;
    int minLen = Integer.MAX_VALUE;
    String result = "";

    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        have.put(c, have.getOrDefault(c, 0) + 1);

        if (need.containsKey(c) && have.get(c).equals(need.get(c))) {
            haveCount++;
        }

        while (haveCount == needCount) {
            // Обновляем результат
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                result = s.substring(left, right + 1);
            }

            // Сужаем окно
            char leftChar = s.charAt(left);
            have.put(leftChar, have.get(leftChar) - 1);
            if (need.containsKey(leftChar) && have.get(leftChar) < need.get(leftChar)) {
                haveCount--;
            }
            left++;
        }
    }

    return result;
}
```

**Сложность:** O(n + m)

---

### Sliding Window Maximum ⭐

**Задача:** Максимум в каждом окне размера k.

**Brute force:** O(n·k)

**Monotonic Deque:** O(n)

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    Deque<Integer> dq = new ArrayDeque<>();  // хранит индексы
    List<Integer> result = new ArrayList<>();

    for (int i = 0; i < nums.length; i++) {
        // Удаляем индексы вне окна
        while (!dq.isEmpty() && dq.peekFirst() < i - k + 1) {
            dq.pollFirst();
        }

        // Удаляем меньшие элементы
        while (!dq.isEmpty() && nums[dq.peekLast()] < nums[i]) {
            dq.pollLast();
        }

        dq.addLast(i);

        // Первое полное окно
        if (i >= k - 1) {
            result.add(nums[dq.peekFirst()]);
        }
    }

    return result.stream().mapToInt(Integer::intValue).toArray();
}
```

**Идея:** Deque хранит индексы в убывающем порядке значений. Front = максимум.

---

### Permutation in String

**Задача:** Проверить, содержит ли s2 перестановку s1.

```java
public boolean checkInclusion(String s1, String s2) {
    if (s1.length() > s2.length()) {
        return false;
    }

    Map<Character, Integer> need = new HashMap<>();
    Map<Character, Integer> have = new HashMap<>();

    for (char c : s1.toCharArray()) {
        need.put(c, need.getOrDefault(c, 0) + 1);
    }
    for (int i = 0; i < s1.length(); i++) {
        char c = s2.charAt(i);
        have.put(c, have.getOrDefault(c, 0) + 1);
    }

    if (have.equals(need)) {
        return true;
    }

    for (int i = s1.length(); i < s2.length(); i++) {
        // Добавляем новый
        char newChar = s2.charAt(i);
        have.put(newChar, have.getOrDefault(newChar, 0) + 1);

        // Убираем старый
        char oldChar = s2.charAt(i - s1.length());
        have.put(oldChar, have.get(oldChar) - 1);
        if (have.get(oldChar) == 0) {
            have.remove(oldChar);
        }

        if (have.equals(need)) {
            return true;
        }
    }

    return false;
}
```

---

### Find All Anagrams in String

**Задача:** Все позиции анаграмм p в s.

```java
public List<Integer> findAnagrams(String s, String p) {
    List<Integer> result = new ArrayList<>();

    if (p.length() > s.length()) {
        return result;
    }

    Map<Character, Integer> need = new HashMap<>();
    Map<Character, Integer> have = new HashMap<>();

    for (char c : p.toCharArray()) {
        need.put(c, need.getOrDefault(c, 0) + 1);
    }
    for (int i = 0; i < p.length(); i++) {
        char c = s.charAt(i);
        have.put(c, have.getOrDefault(c, 0) + 1);
    }

    if (have.equals(need)) {
        result.add(0);
    }

    for (int i = p.length(); i < s.length(); i++) {
        char newChar = s.charAt(i);
        have.put(newChar, have.getOrDefault(newChar, 0) + 1);

        char oldChar = s.charAt(i - p.length());
        have.put(oldChar, have.get(oldChar) - 1);
        if (have.get(oldChar) == 0) {
            have.remove(oldChar);
        }

        if (have.equals(need)) {
            result.add(i - p.length() + 1);
        }
    }

    return result;
}
```

---

### Fruit Into Baskets

**Задача:** Максимальное количество фруктов с 2 типами (= longest subarray with 2 distinct).

Эквивалентно "Longest Substring with At Most K Distinct" с k=2.

---

### Maximum Average Subarray

**Задача:** Максимальное среднее для подмассива размера k.

```java
public double findMaxAverage(int[] nums, int k) {
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += nums[i];
    }
    int maxSum = windowSum;

    for (int i = k; i < nums.length; i++) {
        windowSum += nums[i] - nums[i - k];
        maxSum = Math.max(maxSum, windowSum);
    }

    return (double) maxSum / k;
}
```

---

## Паттерны Sliding Window

### 1. Fixed Size
- Размер k известен
- Один проход: добавить справа, убрать слева

### 2. Variable Size — Shrinkable
- Пока условие нарушено → сужаем
- Типично для "minimum subarray with condition"

### 3. Variable Size — Non-shrinkable
- Не сужаем, только расширяем
- Типично для "maximum subarray with condition"

### 4. With Counter/HashMap
- Отслеживаем частоты символов
- Для задач с "contains all", "at most k distinct"

### 5. With Monotonic Deque
- Для min/max в окне
- O(n) вместо O(n·k)

---

## Типичные задачи

| Задача | Тип |
|--------|-----|
| Max sum subarray size k | Fixed |
| Maximum average subarray | Fixed |
| Sliding window maximum | Fixed + Monotonic Deque |
| Permutation in string | Fixed + Counter |
| Find all anagrams | Fixed + Counter |
| Minimum size subarray sum | Variable (shrink) |
| Longest substring no repeat | Variable + Set/Map |
| Longest with k distinct | Variable + Counter |
| Minimum window substring | Variable + Counter |
| Fruit into baskets | Variable (2 distinct) |

---

## Запомни

- **Fixed:** Инициализируй окно, потом slide
- **Variable:** Расширяй правым, сужай левым
- **Counter/Map:** Для подсчёта символов
- **Monotonic Deque:** Для min/max в окне
- **Сложность:** Обычно O(n) — каждый элемент входит и выходит 1 раз
- **Ключевой вопрос:** Когда сужать окно?
