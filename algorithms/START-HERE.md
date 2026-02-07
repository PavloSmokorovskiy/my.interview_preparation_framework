# START HERE: Algorithms (Quick + Deep Modes)

> **Если цель:** "понимать алгоритмы, которые спрашивают на Google coding interview" — иди по **Deep Mode**.

> **Если цель:** "быстро освежить перед интервью" — используй **Quick Mode**.

---

## Что здесь изменилось

В разделе добавлен отдельный deep-трек:

- `algorithms/DEEP-UNDERSTANDING-PATH.md` — полный маршрут чтения всех файлов, карта теории, чекпоинты понимания.

Этот файл (`START-HERE.md`) теперь только навигация и выбор режима.

---

## Структура раздела

```
algorithms/
│
├── START-HERE.md                       ← Ты здесь
├── DEEP-UNDERSTANDING-PATH.md          ← Глубокий трек (основной)
├── COMPLETE-ALGORITHMS-GUIDE.md        ← Сводный обзор всех алгоритмов
├── LEETCODE-PRACTICE.md                ← Каталог задач по паттернам
├── self-test.md                        ← Проверка понимания
│
├── data-structures/                    ← 7 файлов фундаментальных структур
├── by-category/                        ← 13 файлов алгоритмических категорий
└── cheatsheets/
    ├── big-o-cheatsheet.md
    ├── pattern-recognition.md
    └── quick-reference.md
```

---

## Deep Mode (рекомендуется)

### Для кого

- Ты хочешь **прочитать все файлы** раздела.
- Тебе нужно не "знать названия", а понимать: почему алгоритм работает, где применим, где нет.
- Ты хочешь уверенно отвечать на follow-up вопросы интервьюера.

### Порядок

1. Открой `algorithms/DEEP-UNDERSTANDING-PATH.md`.
2. Иди строго по порядку 27 файлов.
3. После каждого файла отвечай устно:
- Что решает подход?
- Какой инвариант?
- Почему такая сложность?
- Когда этот подход не подходит?

### Definition of Done

- Прочитаны все файлы `algorithms/`.
- `self-test.md`: стабильно 40+/50.
- По Tier 1 алгоритмам (binary search, graph traversal, DP, heap, two pointers, sliding window, trees, hashing) можешь объяснить решение **без кода**.

---

## Quick Mode (если времени мало)

### Шаг 1: Сжатая база

1. `algorithms/COMPLETE-ALGORITHMS-GUIDE.md`
2. `algorithms/cheatsheets/big-o-cheatsheet.md`
3. `algorithms/cheatsheets/pattern-recognition.md`

### Шаг 2: Must-know темы

4. `algorithms/by-category/02-searching.md`
5. `algorithms/by-category/03-graphs.md`
6. `algorithms/by-category/04-trees.md`
7. `algorithms/by-category/05-dynamic-programming.md`
8. `algorithms/by-category/09-two-pointers.md`
9. `algorithms/by-category/10-sliding-window.md`

### Шаг 3: Проверка

10. `algorithms/self-test.md`
11. Слабые темы добрать через соответствующие файлы в `by-category/` и `data-structures/`.

---

## Как читать, чтобы было глубокое понимание (а не механика)

На каждый алгоритм фиксируй 5 пунктов:

1. **Проблема:** какой тип задач он решает.
2. **Идея:** ключевая мысль в 1-2 предложениях.
3. **Инвариант:** что сохраняется истинным на каждом шаге.
4. **Сложность:** откуда берутся `time` и `space`.
5. **Ограничения:** где подход ломается.

Если один из пунктов не можешь объяснить устно, тема ещё не закрыта.

---

## Что обычно спрашивают в Google (ядро)

1. Binary Search + вариации
2. BFS/DFS, shortest path, topological sort
3. Tree/BST traversal и tree-рекурсия
4. HashMap/Set patterns
5. Two Pointers + Sliding Window
6. Heap/Priority Queue
7. Dynamic Programming базовые паттерны

Где читать это ядро:
- `algorithms/by-category/02-searching.md`
- `algorithms/by-category/03-graphs.md`
- `algorithms/by-category/04-trees.md`
- `algorithms/by-category/05-dynamic-programming.md`
- `algorithms/by-category/09-two-pointers.md`
- `algorithms/by-category/10-sliding-window.md`
- `algorithms/data-structures/hash-tables.md`
- `algorithms/data-structures/heaps.md`

---

## Начни сейчас

1. Открой `algorithms/DEEP-UNDERSTANDING-PATH.md`.
2. Пройди Фазу 0 и Фазу 1 полностью.
3. Не переходи к LeetCode, пока не можешь устно объяснить ядро Tier 1.
