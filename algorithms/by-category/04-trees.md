# Алгоритмы на деревьях

## Обзор

| Операция | BST (avg) | BST (worst) | Balanced | Heap |
|----------|-----------|-------------|----------|------|
| Search | O(log n) | O(n) | O(log n) | O(n) |
| Insert | O(log n) | O(n) | O(log n) | O(log n) |
| Delete | O(log n) | O(n) | O(log n) | O(log n) |
| Min/Max | O(log n) | O(n) | O(log n) | O(1) |

---

## Tree Traversals ⭐

### DFS Traversals

**Inorder (Left → Node → Right)**
```java
void inorder(TreeNode node) {
    if (node != null) {
        inorder(node.left);
        visit(node);
        inorder(node.right);
    }
}
```
- **BST:** Даёт отсортированный порядок
- **Применение:** Kth smallest, validate BST

**Preorder (Node → Left → Right)**
```java
void preorder(TreeNode node) {
    if (node != null) {
        visit(node);
        preorder(node.left);
        preorder(node.right);
    }
}
```
- **Применение:** Копирование дерева, сериализация

**Postorder (Left → Right → Node)**
```java
void postorder(TreeNode node) {
    if (node != null) {
        postorder(node.left);
        postorder(node.right);
        visit(node);
    }
}
```
- **Применение:** Удаление дерева, высота, размер

### BFS Traversal

**Level Order**
```java
List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        List<Integer> level = new ArrayList<>();

        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            level.add(node.val);
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        result.add(level);
    }
    return result;
}
```
- **Применение:** По уровням, кратчайший путь в дереве

### Итеративные версии DFS

**Inorder (с стеком):**
```java
List<Integer> inorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    TreeNode curr = root;

    while (!stack.isEmpty() || curr != null) {
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }
        curr = stack.pop();
        result.add(curr.val);
        curr = curr.right;
    }
    return result;
}
```

**Morris Traversal:** O(1) space, без стека (модификация указателей).

---

## Основные алгоритмы

### Maximum Depth (Height)
```java
int maxDepth(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

### Minimum Depth
```java
// BFS лучше — находит первый лист
int minDepth(TreeNode root) {
    if (root == null) return 0;
    if (root.left == null) return 1 + minDepth(root.right);
    if (root.right == null) return 1 + minDepth(root.left);
    return 1 + Math.min(minDepth(root.left), minDepth(root.right));
}
```

### Same Tree
```java
boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) return true;
    if (p == null || q == null) return false;
    return p.val == q.val &&
           isSameTree(p.left, q.left) &&
           isSameTree(p.right, q.right);
}
```

### Symmetric Tree
```java
boolean isSymmetric(TreeNode root) {
    return mirror(root, root);
}

boolean mirror(TreeNode t1, TreeNode t2) {
    if (t1 == null && t2 == null) return true;
    if (t1 == null || t2 == null) return false;
    return t1.val == t2.val &&
           mirror(t1.left, t2.right) &&
           mirror(t1.right, t2.left);
}
```

### Invert Binary Tree
```java
TreeNode invertTree(TreeNode root) {
    if (root != null) {
        TreeNode temp = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(temp);
    }
    return root;
}
```

---

## BST Операции

### Search
```java
TreeNode search(TreeNode root, int val) {
    if (root == null || root.val == val) {
        return root;
    }
    if (val < root.val) {
        return search(root.left, val);
    }
    return search(root.right, val);
}
```

### Insert
```java
TreeNode insert(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val);
    }
    if (val < root.val) {
        root.left = insert(root.left, val);
    } else {
        root.right = insert(root.right, val);
    }
    return root;
}
```

### Delete
```
Три случая:
1. Leaf → просто удаляем
2. Один ребёнок → заменяем на ребёнка
3. Два ребёнка → заменяем на inorder successor (min в правом)
```

### Validate BST ⭐
```java
boolean isValidBST(TreeNode root) {
    return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
}

boolean isValidBST(TreeNode node, long minVal, long maxVal) {
    if (node == null) return true;
    if (node.val <= minVal || node.val >= maxVal) return false;
    return isValidBST(node.left, minVal, node.val) &&
           isValidBST(node.right, node.val, maxVal);
}
```

### Kth Smallest in BST
```java
// Inorder до K
int kthSmallest(TreeNode root, int k) {
    Stack<TreeNode> stack = new Stack<>();
    TreeNode curr = root;

    while (!stack.isEmpty() || curr != null) {
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }
        curr = stack.pop();
        k--;
        if (k == 0) return curr.val;
        curr = curr.right;
    }
    return -1; // not found
}
```

---

## LCA (Lowest Common Ancestor)

### В обычном Binary Tree
```java
TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) {
        return root;
    }
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);

    if (left != null && right != null) {
        return root;  // p и q в разных поддеревьях
    }
    return left != null ? left : right;
}
```

### В BST (используем свойство)
```java
TreeNode lowestCommonAncestorBST(TreeNode root, TreeNode p, TreeNode q) {
    if (p.val < root.val && q.val < root.val) {
        return lowestCommonAncestorBST(root.left, p, q);
    }
    if (p.val > root.val && q.val > root.val) {
        return lowestCommonAncestorBST(root.right, p, q);
    }
    return root;  // split point
}
```

---

## Path Problems

### Path Sum (есть ли путь с суммой)
```java
boolean hasPathSum(TreeNode root, int targetSum) {
    if (root == null) return false;
    if (root.left == null && root.right == null) {
        return root.val == targetSum;
    }
    return hasPathSum(root.left, targetSum - root.val) ||
           hasPathSum(root.right, targetSum - root.val);
}
```

### All Paths with Sum
```java
List<List<Integer>> pathSum(TreeNode root, int targetSum) {
    List<List<Integer>> result = new ArrayList<>();
    dfs(root, targetSum, new ArrayList<>(), result);
    return result;
}

void dfs(TreeNode node, int remaining, List<Integer> path, List<List<Integer>> result) {
    if (node == null) return;

    path.add(node.val);

    if (node.left == null && node.right == null && remaining == node.val) {
        result.add(new ArrayList<>(path));
    }

    dfs(node.left, remaining - node.val, path, result);
    dfs(node.right, remaining - node.val, path, result);

    path.remove(path.size() - 1);
}
```

### Diameter of Binary Tree
```java
int diameter;

int diameterOfBinaryTree(TreeNode root) {
    diameter = 0;
    height(root);
    return diameter;
}

int height(TreeNode node) {
    if (node == null) return 0;
    int left = height(node.left);
    int right = height(node.right);
    diameter = Math.max(diameter, left + right);
    return 1 + Math.max(left, right);
}
```

### Maximum Path Sum
```java
int maxSum;

int maxPathSum(TreeNode root) {
    maxSum = Integer.MIN_VALUE;
    maxGain(root);
    return maxSum;
}

int maxGain(TreeNode node) {
    if (node == null) return 0;

    int left = Math.max(maxGain(node.left), 0);
    int right = Math.max(maxGain(node.right), 0);

    maxSum = Math.max(maxSum, node.val + left + right);

    return node.val + Math.max(left, right);
}
```

---

## Serialize / Deserialize

### Preorder с null markers
```java
// Serialize
String serialize(TreeNode root) {
    StringBuilder sb = new StringBuilder();
    serializeHelper(root, sb);
    return sb.toString();
}

void serializeHelper(TreeNode node, StringBuilder sb) {
    if (node == null) {
        sb.append("#,");
    } else {
        sb.append(node.val).append(",");
        serializeHelper(node.left, sb);
        serializeHelper(node.right, sb);
    }
}

// Deserialize
TreeNode deserialize(String data) {
    Queue<String> queue = new LinkedList<>(Arrays.asList(data.split(",")));
    return deserializeHelper(queue);
}

TreeNode deserializeHelper(Queue<String> queue) {
    String val = queue.poll();
    if (val.equals("#")) {
        return null;
    }
    TreeNode node = new TreeNode(Integer.parseInt(val));
    node.left = deserializeHelper(queue);
    node.right = deserializeHelper(queue);
    return node;
}
```

---

## Construct Tree

### From Preorder and Inorder
```
Preorder: [root, left_subtree, right_subtree]
Inorder:  [left_subtree, root, right_subtree]

1. Первый в preorder = root
2. Находим root в inorder → делит на левое и правое поддерево
3. Рекурсивно строим поддеревья
```

### From Sorted Array to BST
```java
TreeNode sortedArrayToBST(int[] nums) {
    return build(nums, 0, nums.length - 1);
}

TreeNode build(int[] nums, int left, int right) {
    if (left > right) return null;

    int mid = left + (right - left) / 2;
    TreeNode node = new TreeNode(nums[mid]);
    node.left = build(nums, left, mid - 1);
    node.right = build(nums, mid + 1, right);
    return node;
}
```

---

## Типичные задачи

| Задача | Подход |
|--------|--------|
| Maximum depth | DFS: max(left, right) + 1 |
| Minimum depth | BFS (первый лист) |
| Same tree | Рекурсивное сравнение |
| Symmetric tree | Compare mirror |
| Invert tree | Swap children |
| Validate BST | Min/max bounds |
| Kth smallest BST | Inorder до K |
| LCA | Рекурсия или BST свойство |
| Diameter | Max(left_h + right_h) в каждом узле |
| Path sum | DFS с remaining sum |
| Serialize/Deserialize | Preorder + null markers |
| Construct from traversals | Divide по root в inorder |
| BST Iterator | Stack + inorder simulation |

---

## Запомни

- **Traversals:** Inorder = sorted BST, Preorder = copy, Postorder = delete
- **BST property:** Left < Node < Right
- **Validate BST:** Передавай min/max bounds
- **LCA:** В BST используй свойство, в общем — рекурсия
- **Path problems:** DFS с накоплением суммы/пути
- **Diameter:** Отслеживай max(left_h + right_h)
- **Serialize:** Preorder + null markers
- **Construct:** Root из preorder, split по inorder

---

## Deep Theory Layer

### 1) Recursive contract для tree-задач

Хорошая tree-рекурсия начинается с контракта функции:

- "Что именно возвращает функция для поддерева?"

Примеры:

1. Высота поддерева.
2. Максимальная path sum в поддереве.
3. Валидность BST + диапазон.
4. LCA-кандидат.

Без точного контракта рекурсия быстро становится неуправляемой.

### 2) Top-down vs Bottom-up на деревьях

1. Top-down:
- передаём контекст вниз (bounds, prefix sums, depth).

2. Bottom-up:
- агрегируем информацию детей вверх (height, best path, balanced flag).

Многие hard-задачи требуют комбинированного подхода.

### 3) BST validation: формальная причина ошибки "только parent"

Проверка `node.left < node < node.right` локально недостаточна.

Нужно глобальное ограничение диапазоном:

- `min < node.val < max` для каждого узла.

Иначе можно пропустить нарушение в глубине поддерева.

### 4) Traversal как инструмент состояния

1. Inorder:
- проверка отсортированности BST,
- k-th элемент,
- range queries по BST.

2. Postorder:
- вычисление метрик, зависящих от детей.

3. Level-order:
- минимальная глубина,
- задачи по уровням, "первый узел уровня".

### 5) Tree DP (очень частый follow-up)

Суть:

- в каждом узле хранить несколько состояний.

Классика:

- House Robber III: `take(node)` и `skip(node)`.
- Camera Cover: состояния покрытия.

Это мост между "tree" и "DP" мышлением.

### 6) Complexity caveats

В деревьях операции часто записывают как `O(log n)` или `O(h)`.

Важно:

- `h = O(log n)` только при балансировке.
- в худшем BST `h = O(n)`.

### 7) Interview follow-up

1. Рекурсия переполняет стек -> iterative traversal.
2. Много query на одном дереве -> preprocessing.
3. Изменения дерева online -> балансировка/augmented structures.

### 8) Контрольные вопросы

1. Почему inorder в BST отсортирован?
2. В чём разница между depth и height?
3. Почему validate BST требует диапазонов?
4. Когда LCA можно сделать быстрее, чем O(h) per query?
5. Как выбрать traversal под задачу?
