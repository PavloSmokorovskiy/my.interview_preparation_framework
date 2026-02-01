# String Algorithms (Строковые алгоритмы)

## Pattern Matching

### Naive (Brute Force)

**Идея:** Проверяем pattern на каждой позиции text.

```java
public List<Integer> naiveSearch(String text, String pattern) {
    int n = text.length();
    int m = pattern.length();
    List<Integer> positions = new ArrayList<>();

    for (int i = 0; i <= n - m; i++) {
        boolean match = true;
        for (int j = 0; j < m; j++) {
            if (text.charAt(i + j) != pattern.charAt(j)) {
                match = false;
                break;
            }
        }
        if (match) {
            positions.add(i);
        }
    }

    return positions;
}
```

**Сложность:** O(n·m) worst case

---

### KMP (Knuth-Morris-Pratt) ⭐

**Идея:** Не начинаем сначала при несовпадении. Используем информацию о совпавшей части.

**LPS (Longest Proper Prefix which is also Suffix):**
```
pattern = "ABABC"
LPS     = [0, 0, 1, 2, 0]

A: нет proper prefix = suffix → 0
AB: нет → 0
ABA: "A" = prefix и suffix → 1
ABAB: "AB" = prefix и suffix → 2
ABABC: нет → 0
```

**Алгоритм:**
```java
public int[] buildLPS(String pattern) {
    int m = pattern.length();
    int[] lps = new int[m];
    int length = 0;
    int i = 1;

    while (i < m) {
        if (pattern.charAt(i) == pattern.charAt(length)) {
            length++;
            lps[i] = length;
            i++;
        } else {
            if (length != 0) {
                length = lps[length - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }

    return lps;
}

public List<Integer> KMP(String text, String pattern) {
    int n = text.length();
    int m = pattern.length();
    int[] lps = buildLPS(pattern);
    List<Integer> positions = new ArrayList<>();

    int i = 0, j = 0;
    while (i < n) {
        if (text.charAt(i) == pattern.charAt(j)) {
            i++;
            j++;
            if (j == m) {
                positions.add(i - j);
                j = lps[j - 1];
            }
        } else {
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }

    return positions;
}
```

**Сложность:** O(n + m)

---

### Rabin-Karp

**Идея:** Используем хэши для быстрого сравнения. Rolling hash для O(1) обновления.

```java
public List<Integer> rabinKarp(String text, String pattern) {
    int n = text.length();
    int m = pattern.length();
    int d = 256;  // размер алфавита
    int q = 101;  // простое число для mod
    int h = 1;

    // h = d^(m-1) % q
    for (int i = 0; i < m - 1; i++) {
        h = (h * d) % q;
    }

    // Хэш pattern и первого окна
    int pHash = 0;
    int tHash = 0;
    for (int i = 0; i < m; i++) {
        pHash = (d * pHash + pattern.charAt(i)) % q;
        tHash = (d * tHash + text.charAt(i)) % q;
    }

    List<Integer> positions = new ArrayList<>();

    for (int i = 0; i <= n - m; i++) {
        if (pHash == tHash) {
            // Проверяем на коллизию
            if (text.substring(i, i + m).equals(pattern)) {
                positions.add(i);
            }
        }

        // Rolling hash
        if (i < n - m) {
            tHash = (d * (tHash - text.charAt(i) * h) + text.charAt(i + m)) % q;
            if (tHash < 0) {
                tHash += q;
            }
        }
    }

    return positions;
}
```

**Сложность:**
- Average: O(n + m)
- Worst: O(n·m) при много коллизий

**Преимущество:** Хорошо для multiple patterns.

---

### Z-Algorithm

**Идея:** Z[i] = длина наибольшего префикса, совпадающего с подстрокой начиная с i.

```
text = "aabxaab"
Z    = [-, 1, 0, 0, 3, 1, 0]

Для pattern matching:
concat = pattern + "$" + text
Z-array покажет, где pattern встречается
```

**Сложность:** O(n + m)

---

## Palindrome Algorithms

### Check Palindrome

```java
public boolean isPalindrome(String s) {
    return s.equals(new StringBuilder(s).reverse().toString());
}

// Или two pointers:
public boolean isPalindromeTwoPointers(String s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

---

### Longest Palindromic Substring ⭐

**Expand Around Center:**
```java
public String longestPalindrome(String s) {
    if (s == null || s.length() == 0) return "";

    int start = 0, end = 0;

    for (int i = 0; i < s.length(); i++) {
        int len1 = expandAroundCenter(s, i, i);      // Odd length
        int len2 = expandAroundCenter(s, i, i + 1);  // Even length
        int len = Math.max(len1, len2);

        if (len > end - start) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }

    return s.substring(start, end + 1);
}

private int expandAroundCenter(String s, int left, int right) {
    while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
        left--;
        right++;
    }
    return right - left - 1;
}
```

**Сложность:** O(n²)

**Manacher's Algorithm:** O(n), но сложнее.

---

### Longest Palindromic Subsequence

**DP подход:**
```java
public int longestPalindromeSubseq(String s) {
    int n = s.length();
    int[][] dp = new int[n][n];

    for (int i = 0; i < n; i++) {
        dp[i][i] = 1;
    }

    for (int length = 2; length <= n; length++) {
        for (int i = 0; i <= n - length; i++) {
            int j = i + length - 1;
            if (s.charAt(i) == s.charAt(j)) {
                dp[i][j] = dp[i + 1][j - 1] + 2;
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }

    return dp[0][n - 1];
}
```

Или: LCS(s, reverse(s))

---

### Palindrome Partitioning

**Задача:** Минимум разрезов для разбиения на палиндромы.

```java
public int minCut(String s) {
    int n = s.length();

    // isPalindrome[i][j] = true если s[i..j] палиндром
    boolean[][] isPalindrome = new boolean[n][n];
    for (int i = 0; i < n; i++) {
        isPalindrome[i][i] = true;
    }
    for (int length = 2; length <= n; length++) {
        for (int i = 0; i <= n - length; i++) {
            int j = i + length - 1;
            if (s.charAt(i) == s.charAt(j)) {
                isPalindrome[i][j] = (length == 2) || isPalindrome[i + 1][j - 1];
            }
        }
    }

    // dp[i] = min cuts for s[0..i]
    int[] dp = new int[n];
    Arrays.fill(dp, Integer.MAX_VALUE);

    for (int i = 0; i < n; i++) {
        if (isPalindrome[0][i]) {
            dp[i] = 0;
        } else {
            for (int j = 0; j < i; j++) {
                if (isPalindrome[j + 1][i]) {
                    dp[i] = Math.min(dp[i], dp[j] + 1);
                }
            }
        }
    }

    return dp[n - 1];
}
```

---

## String Hashing

### Polynomial Rolling Hash

```
hash(s) = s[0] + s[1]*p + s[2]*p² + ... + s[n-1]*p^(n-1)  mod m

p — простое (обычно 31 или 37)
m — большое простое (10⁹+7)
```

### Rolling Hash Update

```
Для sliding window:
hash(s[i+1..i+k]) = (hash(s[i..i+k-1]) - s[i]) / p + s[i+k] * p^(k-1)
```

### Применения
- Pattern matching (Rabin-Karp)
- Finding duplicate substrings
- Longest common substring

---

## Trie Operations

### Implement Trie

```java
class TrieNode {
    Map<Character, TrieNode> children;
    boolean isEnd;

    public TrieNode() {
        children = new HashMap<>();
        isEnd = false;
    }
}

class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            if (!node.children.containsKey(c)) {
                node.children.put(c, new TrieNode());
            }
            node = node.children.get(c);
        }
        node.isEnd = true;
    }

    public boolean search(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            if (!node.children.containsKey(c)) {
                return false;
            }
            node = node.children.get(c);
        }
        return node.isEnd;
    }

    public boolean startsWith(String prefix) {
        TrieNode node = root;
        for (char c : prefix.toCharArray()) {
            if (!node.children.containsKey(c)) {
                return false;
            }
            node = node.children.get(c);
        }
        return true;
    }
}
```

---

## Classic String Problems

### Anagram Check

```java
public boolean isAnagram(String s, String t) {
    char[] sArr = s.toCharArray();
    char[] tArr = t.toCharArray();
    Arrays.sort(sArr);
    Arrays.sort(tArr);
    return Arrays.equals(sArr, tArr);

    // или с использованием HashMap для подсчета символов
}
```

### Group Anagrams

```java
public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> groups = new HashMap<>();

    for (String s : strs) {
        char[] chars = s.toCharArray();
        Arrays.sort(chars);
        String key = new String(chars);

        groups.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
    }

    return new ArrayList<>(groups.values());
}
```

### Longest Common Prefix

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }

    String prefix = strs[0];
    for (int i = 1; i < strs.length; i++) {
        while (!strs[i].startsWith(prefix)) {
            prefix = prefix.substring(0, prefix.length() - 1);
            if (prefix.isEmpty()) {
                return "";
            }
        }
    }

    return prefix;
}
```

---

### Edit Distance (Levenshtein) ⭐

```java
public int minDistance(String word1, String word2) {
    int m = word1.length();
    int n = word2.length();
    int[][] dp = new int[m + 1][n + 1];

    for (int i = 0; i <= m; i++) {
        dp[i][0] = i;
    }
    for (int j = 0; j <= n; j++) {
        dp[0][j] = j;
    }

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = 1 + Math.min(
                    dp[i - 1][j],      // delete
                    Math.min(
                        dp[i][j - 1],  // insert
                        dp[i - 1][j - 1]  // replace
                    )
                );
            }
        }
    }

    return dp[m][n];
}
```

---

### Word Break

```java
public boolean wordBreak(String s, List<String> wordDict) {
    Set<String> wordSet = new HashSet<>(wordDict);
    int n = s.length();
    boolean[] dp = new boolean[n + 1];
    dp[0] = true;

    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && wordSet.contains(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }

    return dp[n];
}
```

---

## Типичные задачи

| Задача | Подход |
|--------|--------|
| Pattern matching | KMP, Rabin-Karp |
| Longest palindromic substring | Expand around center |
| Longest palindromic subsequence | DP или LCS |
| Edit distance | DP 2D |
| Longest common subsequence | DP 2D |
| Longest common prefix | Iterate and trim |
| Anagram check | Sort или Counter |
| Group anagrams | Sorted string as key |
| Word break | DP + HashSet |
| Implement Trie | TrieNode with children map |
| Word search | Trie + DFS in matrix |

---

## Запомни

- **KMP:** O(n+m), LPS array для skip
- **Rabin-Karp:** Rolling hash, good for multiple patterns
- **Palindrome substring:** Expand around center O(n²)
- **Palindrome subsequence:** DP или LCS(s, reverse(s))
- **Edit distance:** Classic 2D DP
- **Trie:** Prefix searches, autocomplete
- **Hashing:** Для быстрого сравнения подстрок
