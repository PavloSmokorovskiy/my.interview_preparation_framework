# Two Pointers (Два указателя)

## Что это? ⭐

**Идея:** Два указателя, движущиеся по массиву/строке для решения задачи за один проход.

**Когда использовать:**
- Отсортированный массив + поиск пары
- In-place модификация массива
- Подмассив с условием
- Linked list (fast/slow)

---

## Паттерны

### 1. Opposite Direction (навстречу друг другу)

```java
int left = 0;
int right = n - 1;

while (left < right) {
    if (condition) {
        // нашли ответ или сужаем
    } else if (needLarger) {
        left++;
    } else {
        right--;
    }
}
```

**Примеры:**
- Two Sum (sorted)
- 3Sum, 4Sum
- Container with Most Water
- Valid Palindrome
- Reverse String

---

### 2. Same Direction (в одном направлении)

```java
int slow = 0;
int fast = 0;

while (fast < n) {
    if (condition) {
        // что-то делаем с slow
        slow++;
    }
    fast++;
}
```

**Примеры:**
- Remove Duplicates
- Move Zeroes
- Remove Element
- Fast/Slow pointer (linked list)

---

### 3. Two Arrays

```java
int i = 0;  // pointer for arr1
int j = 0;  // pointer for arr2

while (i < arr1.length && j < arr2.length) {
    if (arr1[i] < arr2[j]) {
        i++;
    } else if (arr1[i] > arr2[j]) {
        j++;
    } else {
        // arr1[i] == arr2[j]
        // обрабатываем совпадение
    }
}
```

**Примеры:**
- Merge Two Sorted Arrays
- Intersection of Two Arrays
- Merge Sorted Array (in-place)

---

## Классические задачи

### Two Sum (Sorted Array) ⭐

**Задача:** Найти два числа с суммой target в отсортированном массиве.

```java
public int[] twoSum(int[] numbers, int target) {
    int left = 0;
    int right = numbers.length - 1;

    while (left < right) {
        int currSum = numbers[left] + numbers[right];
        if (currSum == target) {
            return new int[]{left + 1, right + 1};  // 1-indexed
        } else if (currSum < target) {
            left++;
        } else {
            right--;
        }
    }

    return new int[]{};
}
```

**Сложность:** O(n) time, O(1) space

---

### 3Sum ⭐

**Задача:** Найти все уникальные тройки с суммой 0.

```java
public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> result = new ArrayList<>();

    for (int i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;  // skip duplicates
        }

        int left = i + 1;
        int right = nums.length - 1;
        while (left < right) {
            int total = nums[i] + nums[left] + nums[right];
            if (total == 0) {
                result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                while (left < right && nums[left] == nums[left + 1]) {
                    left++;
                }
                while (left < right && nums[right] == nums[right - 1]) {
                    right--;
                }
                left++;
                right--;
            } else if (total < 0) {
                left++;
            } else {
                right--;
            }
        }
    }

    return result;
}
```

**Сложность:** O(n²) time, O(1) space (не считая output)

---

### Container With Most Water ⭐

**Задача:** Максимальная площадь между двумя линиями.

```java
public int maxArea(int[] height) {
    int left = 0;
    int right = height.length - 1;
    int maxWater = 0;

    while (left < right) {
        int width = right - left;
        int h = Math.min(height[left], height[right]);
        maxWater = Math.max(maxWater, width * h);

        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    return maxWater;
}
```

**Идея:** Сужаем со стороны меньшей высоты (большая не улучшит).

**Сложность:** O(n) time, O(1) space

---

### Remove Duplicates from Sorted Array ⭐

**Задача:** In-place удаление дубликатов.

```java
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) {
        return 0;
    }

    int slow = 0;
    for (int fast = 1; fast < nums.length; fast++) {
        if (nums[fast] != nums[slow]) {
            slow++;
            nums[slow] = nums[fast];
        }
    }

    return slow + 1;
}
```

**slow** — позиция последнего уникального элемента.

---

### Move Zeroes

**Задача:** Переместить все нули в конец, сохранив порядок.

```java
public void moveZeroes(int[] nums) {
    int slow = 0;
    for (int fast = 0; fast < nums.length; fast++) {
        if (nums[fast] != 0) {
            int temp = nums[slow];
            nums[slow] = nums[fast];
            nums[fast] = temp;
            slow++;
        }
    }
}
```

---

### Valid Palindrome

**Задача:** Проверить, что строка — палиндром (игнорируя не-alphanumeric).

```java
public boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;

    while (left < right) {
        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
            left++;
        }
        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
            right--;
        }

        if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
            return false;
        }

        left++;
        right--;
    }

    return true;
}
```

---

### Trapping Rain Water ⭐

**Задача:** Сколько воды можно собрать между столбиками.

```java
public int trap(int[] height) {
    if (height == null || height.length == 0) {
        return 0;
    }

    int left = 0;
    int right = height.length - 1;
    int leftMax = 0;
    int rightMax = 0;
    int water = 0;

    while (left < right) {
        if (height[left] < height[right]) {
            if (height[left] >= leftMax) {
                leftMax = height[left];
            } else {
                water += leftMax - height[left];
            }
            left++;
        } else {
            if (height[right] >= rightMax) {
                rightMax = height[right];
            } else {
                water += rightMax - height[right];
            }
            right--;
        }
    }

    return water;
}
```

**Идея:** Вода определяется меньшей из двух сторон.

---

### Merge Sorted Array

**Задача:** Слить nums2 в nums1 (nums1 имеет место).

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int p1 = m - 1;
    int p2 = n - 1;
    int p = m + n - 1;

    while (p2 >= 0) {
        if (p1 >= 0 && nums1[p1] > nums2[p2]) {
            nums1[p] = nums1[p1];
            p1--;
        } else {
            nums1[p] = nums2[p2];
            p2--;
        }
        p--;
    }
}
```

**Идея:** Заполняем с конца, чтобы не затереть данные.

---

### Intersection of Two Arrays

**Задача:** Найти общие элементы.

```java
public int[] intersection(int[] nums1, int[] nums2) {
    Arrays.sort(nums1);
    Arrays.sort(nums2);
    List<Integer> result = new ArrayList<>();
    int i = 0;
    int j = 0;

    while (i < nums1.length && j < nums2.length) {
        if (nums1[i] < nums2[j]) {
            i++;
        } else if (nums1[i] > nums2[j]) {
            j++;
        } else {
            if (result.isEmpty() || result.get(result.size() - 1) != nums1[i]) {
                result.add(nums1[i]);
            }
            i++;
            j++;
        }
    }

    return result.stream().mapToInt(Integer::intValue).toArray();
}
```

Альтернатива: HashSet — O(n+m) без сортировки.

---

### Sort Colors (Dutch National Flag) ⭐

**Задача:** Сортировка массива из 0, 1, 2 за один проход.

```java
public void sortColors(int[] nums) {
    int low = 0;
    int mid = 0;
    int high = nums.length - 1;

    while (mid <= high) {
        if (nums[mid] == 0) {
            int temp = nums[low];
            nums[low] = nums[mid];
            nums[mid] = temp;
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            mid++;
        } else {  // nums[mid] == 2
            int temp = nums[mid];
            nums[mid] = nums[high];
            nums[high] = temp;
            high--;
        }
    }
}
```

**Три указателя:** low (граница 0), mid (текущий), high (граница 2).

---

## Linked List: Fast & Slow Pointers

### Detect Cycle

```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            return true;
        }
    }
    return false;
}
```

### Find Cycle Start

```java
public ListNode detectCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            slow = head;
            while (slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;
        }
    }
    return null;
}
```

### Find Middle

```java
public ListNode middleNode(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```

---

## Типичные задачи

| Задача | Паттерн |
|--------|---------|
| Two Sum (sorted) | Opposite direction |
| 3Sum, 4Sum | Fix one + two pointers |
| Container with Most Water | Opposite, shrink smaller |
| Valid Palindrome | Opposite direction |
| Remove Duplicates | Same direction |
| Move Zeroes | Same direction |
| Merge Sorted Array | Two arrays, fill from end |
| Intersection | Two arrays |
| Sort Colors | Three pointers |
| Linked List Cycle | Fast/slow |
| Find Middle | Fast/slow |
| Trapping Rain Water | Opposite + track max |

---

## Запомни

- **Opposite direction:** Для отсортированных массивов, поиск пар
- **Same direction:** In-place модификация, fast/slow
- **Two arrays:** Merge, intersection отсортированных
- **Ключ:** Определи как двигать указатели
- **Сложность:** Обычно O(n) time, O(1) space
