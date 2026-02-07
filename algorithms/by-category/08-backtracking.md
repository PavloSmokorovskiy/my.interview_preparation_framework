# Backtracking (Перебор с возвратом)

## Что это? ⭐

**Идея:** Строим решение по частям. Если текущий путь не ведёт к решению — откатываемся (backtrack) и пробуем другой.

**Метафора:** Лабиринт. Идём вперёд, упёрлись в стену — возвращаемся и пробуем другой путь.

---

## Template (шаблон)

```java
void backtrack(State state, List<Choice> choices) {
    if (isSolution(state)) {
        saveSolution(state);
        return;
    }

    for (Choice choice : choices) {
        if (isValid(choice, state)) {
            makeChoice(choice, state);
            backtrack(state, remainingChoices);
            undoChoice(choice, state);  // backtrack!
        }
    }
}
```

**Ключевые элементы:**
1. **Base case:** Когда нашли решение
2. **Choices:** Какие варианты на текущем шаге
3. **Constraints:** Какие варианты валидны
4. **Goal:** Когда state — решение

---

## Backtracking vs DFS

**Backtracking** = DFS + pruning (отсечение)

- DFS обходит всё дерево решений
- Backtracking отсекает невозможные ветки рано

---

## Классические задачи

### Subsets (все подмножества) ⭐

**Задача:** Все подмножества множества [1,2,3]

**Идея:** Для каждого элемента — включать или не включать.

```
nums = [1, 2, 3]

                      []
              /                \
           [1]                  []
         /     \              /    \
      [1,2]    [1]          [2]    []
      /   \    /  \         / \    / \
  [1,2,3][1,2][1,3][1]  [2,3][2][3] []

Результат: [], [1], [2], [3], [1,2], [1,3], [2,3], [1,2,3]
```

```java
List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, 0, new ArrayList<>(), result);
    return result;
}

void backtrack(int[] nums, int start, List<Integer> path, List<List<Integer>> result) {
    result.add(new ArrayList<>(path));  // копируем текущее подмножество

    for (int i = start; i < nums.length; i++) {
        path.add(nums[i]);
        backtrack(nums, i + 1, path, result);
        path.remove(path.size() - 1);  // backtrack
    }
}
```

**Количество:** 2^n подмножеств

---

### Permutations (все перестановки) ⭐

**Задача:** Все перестановки [1,2,3]

**Идея:** На каждую позицию ставим каждый неиспользованный элемент.

```java
List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    boolean[] used = new boolean[nums.length];
    backtrack(nums, new ArrayList<>(), used, result);
    return result;
}

void backtrack(int[] nums, List<Integer> path, boolean[] used, List<List<Integer>> result) {
    if (path.size() == nums.length) {
        result.add(new ArrayList<>(path));
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;

        used[i] = true;
        path.add(nums[i]);
        backtrack(nums, path, used, result);
        path.remove(path.size() - 1);
        used[i] = false;
    }
}
```

**Количество:** n! перестановок

---

### Combinations (все комбинации) ⭐

**Задача:** Все комбинации из k элементов из [1..n]

```java
List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(1, n, k, new ArrayList<>(), result);
    return result;
}

void backtrack(int start, int n, int k, List<Integer> path, List<List<Integer>> result) {
    if (path.size() == k) {
        result.add(new ArrayList<>(path));
        return;
    }

    for (int i = start; i <= n; i++) {
        path.add(i);
        backtrack(i + 1, n, k, path, result);
        path.remove(path.size() - 1);
    }
}
```

**Количество:** C(n, k) = n! / (k!(n-k)!)

---

### N-Queens ⭐

**Задача:** Расставить N ферзей на N×N доске без атак.

```java
List<List<String>> solveNQueens(int n) {
    List<List<String>> result = new ArrayList<>();
    char[][] board = new char[n][n];
    for (char[] row : board) Arrays.fill(row, '.');
    backtrack(board, 0, result);
    return result;
}

void backtrack(char[][] board, int row, List<List<String>> result) {
    if (row == board.length) {
        result.add(construct(board));
        return;
    }

    for (int col = 0; col < board.length; col++) {
        if (isValid(board, row, col)) {
            board[row][col] = 'Q';
            backtrack(board, row + 1, result);
            board[row][col] = '.';  // backtrack
        }
    }
}

boolean isValid(char[][] board, int row, int col) {
    int n = board.length;

    // Проверяем столбец
    for (int i = 0; i < row; i++) {
        if (board[i][col] == 'Q') return false;
    }

    // Проверяем диагональ влево-вверх
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q') return false;
    }

    // Проверяем диагональ вправо-вверх
    for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
        if (board[i][j] == 'Q') return false;
    }

    return true;
}

List<String> construct(char[][] board) {
    List<String> result = new ArrayList<>();
    for (char[] row : board) {
        result.add(new String(row));
    }
    return result;
}
```

---

### Word Search

**Задача:** Найти слово в матрице (можно идти вверх/вниз/влево/вправо).

```java
boolean exist(char[][] board, String word) {
    int rows = board.length, cols = board[0].length;

    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            if (backtrack(board, word, r, c, 0)) {
                return true;
            }
        }
    }
    return false;
}

boolean backtrack(char[][] board, String word, int r, int c, int index) {
    if (index == word.length()) return true;

    if (r < 0 || r >= board.length || c < 0 || c >= board[0].length) {
        return false;
    }
    if (board[r][c] != word.charAt(index)) return false;

    char temp = board[r][c];
    board[r][c] = '#';  // помечаем как посещённый

    boolean found = backtrack(board, word, r + 1, c, index + 1) ||
                    backtrack(board, word, r - 1, c, index + 1) ||
                    backtrack(board, word, r, c + 1, index + 1) ||
                    backtrack(board, word, r, c - 1, index + 1);

    board[r][c] = temp;  // backtrack
    return found;
}
```

---

### Combination Sum

**Задача:** Найти все комбинации чисел, дающие target (можно использовать число несколько раз).

```java
List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(candidates, 0, target, new ArrayList<>(), result);
    return result;
}

void backtrack(int[] candidates, int start, int remaining,
               List<Integer> path, List<List<Integer>> result) {
    if (remaining == 0) {
        result.add(new ArrayList<>(path));
        return;
    }
    if (remaining < 0) return;

    for (int i = start; i < candidates.length; i++) {
        path.add(candidates[i]);
        backtrack(candidates, i, remaining - candidates[i], path, result);  // i, не i+1 — можно повторять
        path.remove(path.size() - 1);
    }
}
```

---

### Palindrome Partitioning

**Задача:** Разбить строку на палиндромы всеми способами.

```java
List<List<String>> partition(String s) {
    List<List<String>> result = new ArrayList<>();
    backtrack(s, 0, new ArrayList<>(), result);
    return result;
}

void backtrack(String s, int start, List<String> path, List<List<String>> result) {
    if (start == s.length()) {
        result.add(new ArrayList<>(path));
        return;
    }

    for (int end = start + 1; end <= s.length(); end++) {
        String sub = s.substring(start, end);
        if (isPalindrome(sub)) {
            path.add(sub);
            backtrack(s, end, path, result);
            path.remove(path.size() - 1);
        }
    }
}

boolean isPalindrome(String s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        if (s.charAt(left++) != s.charAt(right--)) return false;
    }
    return true;
}
```

---

### Generate Parentheses

**Задача:** Все валидные комбинации n пар скобок.

```java
List<String> generateParenthesis(int n) {
    List<String> result = new ArrayList<>();
    backtrack(n, 0, 0, new StringBuilder(), result);
    return result;
}

void backtrack(int n, int open, int close, StringBuilder path, List<String> result) {
    if (path.length() == 2 * n) {
        result.add(path.toString());
        return;
    }

    if (open < n) {
        path.append('(');
        backtrack(n, open + 1, close, path, result);
        path.deleteCharAt(path.length() - 1);
    }

    if (close < open) {
        path.append(')');
        backtrack(n, open, close + 1, path, result);
        path.deleteCharAt(path.length() - 1);
    }
}
```

---

### Sudoku Solver

**Задача:** Решить судоку.

```java
void solveSudoku(char[][] board) {
    backtrack(board);
}

boolean backtrack(char[][] board) {
    for (int r = 0; r < 9; r++) {
        for (int c = 0; c < 9; c++) {
            if (board[r][c] == '.') {
                for (char num = '1'; num <= '9'; num++) {
                    if (isValid(board, r, c, num)) {
                        board[r][c] = num;
                        if (backtrack(board)) return true;
                        board[r][c] = '.';  // backtrack
                    }
                }
                return false;  // никакое число не подошло
            }
        }
    }
    return true;  // все заполнено
}

boolean isValid(char[][] board, int row, int col, char num) {
    for (int i = 0; i < 9; i++) {
        // Проверяем строку и столбец
        if (board[row][i] == num || board[i][col] == num) return false;

        // Проверяем 3x3 квадрат
        int boxRow = 3 * (row / 3) + i / 3;
        int boxCol = 3 * (col / 3) + i % 3;
        if (board[boxRow][boxCol] == num) return false;
    }
    return true;
}
```

---

## Оптимизации

### 1. Pruning (отсечение)
Отсекаем ветки, которые точно не дадут решения.

```java
// В combinationSum:
if (remaining < 0) return;  // нет смысла продолжать

// В N-Queens:
if (isValid(row, col)) {  // проверяем перед размещением
```

### 2. Ordering (порядок выбора)
Пробуем более вероятные варианты первыми.

### 3. Constraint Propagation
Используем ограничения для уменьшения пространства поиска.

---

## Сложность

Обычно **экспоненциальная**:
- Subsets: O(2^n)
- Permutations: O(n!)
- Combinations C(n,k): O(C(n,k))
- N-Queens: O(n!)

Backtracking не улучшает worst case, но pruning сильно ускоряет на практике.

---

## Типичные задачи

| Задача | Тип |
|--------|-----|
| Subsets | Include/exclude |
| Permutations | Arrange all |
| Combinations | Choose k |
| Combination Sum | Sum to target |
| N-Queens | Constraint placement |
| Word Search | Path in grid |
| Palindrome Partitioning | All partitions |
| Generate Parentheses | Valid sequences |
| Sudoku Solver | Fill grid |
| Letter Combinations of Phone | Cartesian product |

---

## Запомни

- **Backtracking:** Build → Check → Undo
- **Template:** makeChoice → recurse → undoChoice
- **Pruning:** Отсекай невозможное рано
- **Сложность:** Обычно экспоненциальная
- **Отличие от DFS:** Backtracking отсекает ветки
- **Ключевое:** Правильно определить choices и constraints

---

## Deep Theory Layer

### 1) State-space tree

Backtracking исследует дерево состояний:

1. Узел - частичное решение.
2. Рёбра - допустимые choices.
3. Лист - полное решение или тупик.

Понимание формы этого дерева даёт реальную оценку сложности.

### 2) Branching factor и depth

Грубая оценка:

- `O(b^d)` где `b` - branching factor, `d` - глубина.

Оптимизация = уменьшать `b` и/или `d` раньше.

### 3) Pruning как источник практической скорости

Pruning корректен, если доказываем:

- отсечённая ветка не может содержать валидный/лучший ответ.

Виды pruning:

1. Feasibility pruning (нарушены constraints).
2. Bound pruning (даже оптимистичная оценка хуже текущего best).
3. Symmetry pruning (эквивалентные состояния).

### 4) Canonical ordering

Чтобы не генерировать дубликаты:

- фиксируем порядок choices,
- ограничиваем повторный выбор через индекс/visited rules.

Это критично для combinations/permutations with duplicates.

### 5) Backtracking vs Branch and Bound

- Backtracking: обычно "найти все" валидные.
- Branch and Bound: optimization + upper/lower bounds.

На интервью часто можно улучшить backtracking до branch-and-bound идеями.

### 6) Ошибки состояния

1. Forget undo -> загрязнение следующей ветки.
2. Shared mutable objects без копии/отката.
3. Неправильный termination condition.

### 7) Interview follow-up

1. Как убрать дубликаты решений?
2. Как добавить pruning для ускорения?
3. Можно ли перейти на iterative stack без потери читабельности?
4. Можно ли превратить в DP/bitmask DP?

### 8) Контрольные вопросы

1. Почему backtracking экспоненциальный даже с pruning?
2. Как доказать корректность pruning-условия?
3. Чем combinations отличаются от permutations на уровне state?
4. Как определить, что порядок choices важен?
5. Когда backtracking хуже DP фундаментально?
