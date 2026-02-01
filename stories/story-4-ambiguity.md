# История 4: Решение в неопределённости

Пример истории о работе с неполной информацией и меняющимися требованиями.

---

## Метаданные

**Название истории:** Запуск MVP при неопределённых требованиях

**Категория:** Ambiguity

**Покрывает черты Googleyness:**
- [x] Ambiguity
- [ ] Humility
- [x] Bias for Action
- [ ] Doing the Right Thing
- [x] Ownership
- [ ] High Standards
- [x] Innovation
- [ ] Collaboration

**Покрывает Leadership сигналы:**
- [x] Initiative
- [ ] Influence
- [ ] Mentorship
- [x] Driving Results
- [x] Decision Making

**Подходит для вопросов:**
1. Tell me about a time you had to make a decision with incomplete information
2. Describe a project where requirements were unclear
3. How do you handle ambiguity?

---

## STAR структура

### Situation (30 секунд)

**Контекст:**
- Компания/проект: [Ваша компания], новый B2B продукт
- Ваша роль: Software Engineer, единственный инженер на проекте
- Размер команды: Я + PM + дизайнер
- Временной период: 3 месяца

**Background:**
Компания решила выйти на новый B2B сегмент. PM собрал первичные интервью с потенциальными клиентами, но требования были размыты: "нужен dashboard для analytics, но мы не уверены какие метрики важны". Нужно было что-то показать клиентам через 6 недель.

---

### Task (15 секунд)

**Ваша ответственность:**
Спроектировать и реализовать MVP, который можно показать клиентам для валидации идеи.

**Constraints:**
- 6 недель до demo
- Неясно, какие фичи нужны
- Один инженер (я)
- Нет существующей инфраструктуры для B2B

**Почему это было сложно:**
Классическая проблема: требования неясны, но дедлайн фиксирован. Можно потратить 6 недель на неправильную вещь.

---

### Action (1.5 минуты)

**Шаг 1: Определение constraints и uncertainties**
Вместо того чтобы ждать чётких требований, я составил список того, что мы ЗНАЕМ (demo через 6 недель, нужен dashboard, целевые клиенты — enterprise) и чего НЕ ЗНАЕМ (конкретные метрики, интеграции, workflow).

**Шаг 2: Принцип "reversible decisions"**
Разделил решения на reversible (можно изменить) и irreversible (сложно изменить). Irreversible: выбор технологий, data model structure. Reversible: UI, конкретные метрики, визуализации.

**Шаг 3: Создание гибкой архитектуры**
Спроектировал систему так, чтобы легко добавлять новые метрики: generic data pipeline + configurable dashboard. Потратил первые 2 недели на infrastructure, а не на конкретные фичи.

**Шаг 4: Rapid iteration с клиентами**
Предложил PM организовать weekly demos с 2-3 клиентами начиная с недели 3. Даже с minimal функциональностью это давало фидбек.

**Шаг 5: Prioritization на основе фидбека**
После каждого demo приоритизировали: что клиенты хотели vs. что ожидали vs. что удивило бы. Перестраивали backlog каждую неделю.

**Как справился с неопределённостью:**
Ключевое решение: вместо того чтобы пытаться угадать правильные фичи заранее, построить систему, которая позволяет быстро менять фичи. Инвестиция в flexibility вместо конкретных features.

---

### Result (30 секунд)

**Количественные результаты:**
- MVP готов за 5.5 недель
- 3 клиента согласились на paid pilot
- Pivot на 30% фичей после клиентского фидбека (правильное решение)

**Качественные результаты:**
- Подход "flexible architecture first" стал стандартом для новых продуктов
- PM отметил, что engineering не было bottleneck несмотря на неопределённость

**Broader impact:**
- B2B продукт стал profitable через 8 месяцев
- Архитектурные решения позволили масштабировать без rewrite

---

### Learning

**Главный урок:**
При неопределённости инвестируй в flexibility, а не в угадывание правильного ответа. Дешевле построить систему, которая легко меняется, чем угадать requirements с первого раза.

**Framework для ambiguity:**
1. Определи что знаешь и чего не знаешь
2. Раздели решения на reversible и irreversible
3. Для irreversible — выбирай гибкие варианты
4. Для reversible — действуй быстро, итерируй
5. Сокращай feedback loop (чаще показывай клиентам)

**Как применяете сейчас:**
При любом новом проекте начинаю с mapping uncertainties. Это определяет где инвестировать в flexibility, а где можно действовать быстро.

---

## Подготовка к Follow-up

### О решениях
**Вопрос:** "How did you decide what to build first?"
**Ответ:** Приоритизировал infrastructure над features. Рассуждение: фичи изменятся (high uncertainty), infrastructure останется (low uncertainty). Лучше потратить время на stable foundation.

### О рисках
**Вопрос:** "What if you had built the wrong thing?"
**Ответ:** Именно поэтому настоял на weekly demos с клиентами. Maximum exposure time — 1 неделя работы. Если бы шли 6 недель без фидбека, риск был бы намного выше.

### Об альтернативах
**Вопрос:** "Could you have waited for clearer requirements?"
**Ответ:** Дедлайн был фиксирован — partnership agreement. Ждать было не option. Вопрос был как снизить risk при работе с uncertainty, а не избежать uncertainty.

### О команде
**Вопрос:** "How did you work with PM on priorities?"
**Ответ:** Daily standups + shared doc с "known/unknown" matrix. После каждого клиентского demo — совместная сессия репрайоритизации. PM владел "что", я владел "как".

---

## Тайминг

| Секция | Целевое время | Реальное время |
|--------|---------------|----------------|
| Situation | 30 сек | |
| Task | 15 сек | |
| Action | 90 сек | |
| Result | 30 сек | |
| **Итого** | **2:45** | |

---

## Версия для быстрого повторения

**One-liner:** Запустил B2B MVP за 6 недель при неясных требованиях, инвестируя в flexible architecture и короткие feedback loops с клиентами.

**Ключевые точки:**
1. Unclear requirements, fixed deadline, один инженер
2. Classified decisions (reversible/irreversible), built for flexibility, weekly client demos
3. MVP готов, 3 paid pilots, 30% features pivot (validated by customers)
4. Урок: при uncertainty — инвестируй в flexibility, не в угадывание
