# Trees & Tries

## Binary Tree

### Что это
Дерево, где каждый узел имеет не более двух детей (left, right).

```
       1
      / \
     2   3
    / \   \
   4   5   6
```

### Терминология
- **Root:** Корень (узел без родителя)
- **Leaf:** Лист (узел без детей)
- **Height:** Длина пути от узла до самого дальнего листа
- **Depth:** Длина пути от корня до узла
- **Level:** Depth + 1

### Типы Binary Trees
- **Full:** Каждый узел имеет 0 или 2 детей
- **Complete:** Все уровни заполнены, кроме последнего (слева направо)
- **Perfect:** Все листья на одном уровне
- **Balanced:** Разница высот поддеревьев ≤ 1

---

## Binary Tree Traversals ⭐

### DFS Traversals

**Inorder (Left → Root → Right)**
```
       1              Output: 4, 2, 5, 1, 3, 6
      / \
     2   3
    / \   \
   4   5   6
```
**Применение:** BST в отсортированном порядке

**Preorder (Root → Left → Right)**
```
Output: 1, 2, 4, 5, 3, 6
```
**Применение:** Копирование дерева, сериализация

**Postorder (Left → Right → Root)**
```
Output: 4, 5, 2, 6, 3, 1
```
**Применение:** Удаление дерева, вычисление высоты

### BFS Traversal

**Level Order**
```
Output: 1, 2, 3, 4, 5, 6
```
**Применение:** Level-by-level, кратчайший путь в дереве

---

## Binary Search Tree (BST)

### Свойство
Для каждого узла: left < node < right (все значения в левом поддереве меньше, в правом — больше).

### Операции

| Операция | Average | Worst |
|----------|---------|-------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Min/Max | O(log n) | O(n) |

**Worst case:** Вырожденное дерево (linked list).

### Inorder = Sorted
Inorder traversal BST даёт отсортированную последовательность.

### Deletion в BST
1. **Leaf:** Просто удаляем
2. **One child:** Заменяем на ребёнка
3. **Two children:** Заменяем на inorder successor (min в правом поддереве) или predecessor

---

## Balanced BST (концепт)

### AVL Tree
- Строго сбалансировано: |height(left) - height(right)| ≤ 1
- Ротации при нарушении
- Быстрый поиск, но дорогая вставка/удаление

### Red-Black Tree
- Менее строгий баланс
- Используется в std::map, TreeMap, TreeSet
- Гарантия O(log n) для всех операций

**Для интервью:** Знать что существуют, дают O(log n), детали реализации обычно не спрашивают.

---

## Trie (Prefix Tree) ⭐

### Что это
Дерево для хранения строк, где путь от корня — префикс.

```
        root
       /    \
      a      b
     / \      \
    p   n      a
   / \   \      \
  p   e   t      t
 /
l
e

Слова: apple, ape, ant, bat
```

### Структура узла
```
TrieNode {
    children: Map<char, TrieNode>
    isEndOfWord: boolean
}
```

### Операции

| Операция | Time |
|----------|------|
| Insert | O(m) |
| Search | O(m) |
| Prefix Search | O(m) |
| Delete | O(m) |

где m — длина слова.

### Когда использовать
- **Autocomplete:** Все слова с префиксом
- **Spell checker:** Проверка и suggestions
- **Word search in matrix:** Быстрая проверка префиксов
- **IP routing:** Longest prefix match
- **Dictionary:** Быстрый поиск слов

### Trie vs HashMap

| Аспект | Trie | HashMap |
|--------|------|---------|
| Search word | O(m) | O(m) для hash |
| Prefix search | O(m) | O(n) все слова |
| Space | Много (каждый символ) | Компактнее |
| Sorted iteration | Да | Нет |

**Trie лучше когда:** Нужны prefix queries, autocomplete.

---

## Segment Tree

### Что это
Дерево для range queries и point updates.

```
            [0-7]
           /     \
      [0-3]       [4-7]
      /   \       /   \
   [0-1] [2-3] [4-5] [6-7]
   / \   / \   / \   / \
  [0][1][2][3][4][5][6][7]
```

### Операции

| Операция | Time |
|----------|------|
| Build | O(n) |
| Query (range) | O(log n) |
| Update (point) | O(log n) |

### Когда использовать
- Range sum/min/max queries
- С point updates
- Range updates с lazy propagation

### Segment Tree vs Fenwick Tree
- Segment Tree: более гибкий (min, max, sum, gcd...)
- Fenwick Tree: проще, только prefix sums, меньше памяти

---

## Fenwick Tree (Binary Indexed Tree)

### Что это
Компактная структура для prefix sum queries и point updates.

### Операции

| Операция | Time |
|----------|------|
| Build | O(n) или O(n log n) |
| Update | O(log n) |
| Prefix Sum | O(log n) |
| Range Sum | O(log n) |

### Идея
Использует бинарное представление индексов для хитрого разбиения на диапазоны.

**Проще в реализации** чем Segment Tree.

---

## Типичные задачи

### Binary Tree

| Задача | Подход |
|--------|--------|
| Maximum depth | DFS: max(left, right) + 1 |
| Minimum depth | BFS (первый лист) или DFS |
| Same tree | DFS рекурсивно |
| Symmetric tree | Compare left/right |
| Invert tree | Swap left/right рекурсивно |
| LCA | DFS с проверкой наличия |
| Diameter | DFS: max(left_height + right_height) |
| Path sum | DFS с running sum |
| Serialize/Deserialize | Preorder с null markers |

### BST

| Задача | Подход |
|--------|--------|
| Validate BST | DFS с min/max bounds |
| Kth smallest | Inorder до K |
| LCA in BST | Используем свойство BST |
| Convert sorted array to BST | Середина = root, рекурсия |
| BST iterator | Stack + inorder simulation |

### Trie

| Задача | Подход |
|--------|--------|
| Implement Trie | Insert/Search/StartsWith |
| Word Search II | Trie + DFS в матрице |
| Autocomplete | Prefix search + DFS |
| Replace Words | Trie с корнями, находим shortest |

---

## Подводные камни

### 1. BST validation
```
# Неправильно: только сравниваем с parent
# Правильно: передаём допустимый диапазон (min, max)
```

### 2. Tree vs null check
Всегда проверяй на null перед доступом к children!

### 3. Height vs Depth
- Height: от узла вниз к листу
- Depth: от корня вниз к узлу

### 4. Trie memory
Trie может использовать много памяти при большом алфавите. Рассмотри HashMap вместо массива для children.

---

## Запомни

- **Binary Tree:** Max 2 детей, traversals (in/pre/post/level)
- **BST:** Left < Node < Right, inorder = sorted
- **Balanced (AVL, RB):** O(log n) гарантированно
- **Trie:** Префиксы, O(m) операции, autocomplete
- **Segment Tree:** Range queries + point updates, O(log n)
- **Fenwick:** Prefix sums, проще segment tree
