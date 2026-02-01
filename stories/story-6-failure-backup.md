# История 6: Неудача (Backup) — Неправильное решение

Backup история для вопросов о failures, когда основная история #3 не подходит.

---

## Метаданные

**Название истории:** Отменённая фича из-за неправильного понимания требований

**Категория:** Failure (Backup)

**Покрывает черты Googleyness:**
- [ ] Ambiguity
- [x] Humility
- [ ] Bias for Action
- [x] Doing the Right Thing
- [x] Ownership
- [ ] High Standards
- [ ] Innovation
- [x] Collaboration

**Покрывает Leadership сигналы:**
- [ ] Initiative
- [ ] Influence
- [ ] Mentorship
- [ ] Driving Results
- [x] Decision Making

**Подходит для вопросов:**
1. Tell me about a time a project failed
2. Describe when you made a wrong decision
3. Tell me about a time you wasted effort
4. What's a professional regret?

**Когда использовать вместо #3:**
- Если вопрос про "wrong direction", не про "technical mistake"
- Если вопрос про работу с stakeholders/requirements
- Если уже использовал #3 на другой вопрос

---

## STAR структура

### Situation (30 секунд)

**Контекст:**
- Компания/проект: [Ваша компания], B2B analytics product
- Ваша роль: Software Engineer
- Размер команды: 4 человека
- Временной период: 3 месяца

**Background:**
Мы получили feedback от крупного клиента, что им нужна "real-time alerting feature". PM интерпретировал это как приоритет, и мы начали разработку. Я был lead разработчиком на этой фиче.

---

### Task (15 секунд)

**Ваша ответственность:**
Спроектировать и реализовать alerting систему за 3 месяца.

**Constraints:**
- Крупный клиент, важный для бизнеса
- Deadline связан с их renewal date
- Команда работала параллельно с другими задачами

**Почему это было сложно:**
Требования казались понятными, но на самом деле мы не задали достаточно вопросов.

---

### Action (1.5 минуты)

**Шаг 1: Начало работы**
Я погрузился в техническую реализацию. Спроектировал streaming architecture, интегрировал с messaging system, написал alert rules engine. Работа шла хорошо технически.

**Шаг 2: Первые сомнения**
На середине проекта я заметил, что PM стал уклончив в ответах на детальные вопросы о use cases. Но я продолжил — deadline давил, и я доверял первоначальным requirements.

**Шаг 3: Демо клиенту**
Через 2.5 месяца мы показали demo. Реакция была неожиданной: "Это не совсем то, что нам нужно. Нам нужны weekly digest reports, не real-time alerts."

Оказалось, что "alerting" в их понимании — это periodic notifications о трендах, не instant alerts о событиях.

**Шаг 4: Мой ответ**
Вместо blame на PM, я взял часть ответственности:
- Я мог задать больше вопросов в начале
- Я видел warning signs и проигнорировал их
- Я как technical lead должен был настоять на customer validation раньше

Я предложил pivot: использовать часть alerting infrastructure для digest system. Мы смогли сохранить 30% работы.

---

### Result (30 секунд)

**Количественные результаты:**
- ~70% работы (2 месяца) было потеряно
- Digest feature доставили с 3-недельным опозданием
- Клиент остался, но был недоволен процессом

**Качественные результаты:**
- Я инициировал изменение процесса: теперь требуем customer validation перед началом серьёзной разработки
- Создал checklist "before starting a feature" с обязательными вопросами

**Broader impact:**
- Новый процесс предотвратил минимум 2 похожие ситуации за следующий год

---

### Learning

**Главный урок:**
"Clear requirements" — это иллюзия. Как инженер, я отвечаю за то, чтобы понять problem, не просто выполнить task. Если что-то кажется неясным — это red flag, не minor concern.

**Что бы сделали иначе:**
Настоял бы на customer interview в первую неделю, даже если PM сказал "requirements понятны".

**Как применяете сейчас:**
Перед каждой большой фичей спрашиваю: "Кто последний раз говорил с клиентом об этом? Могу ли я увидеть записи?"

---

## Подготовка к Follow-up

### Blame
**Вопрос:** "Was this the PM's fault?"
**Ответ:** PM сделал ошибку в interpretation, но я как technical lead тоже несу ответственность. Я видел warning signs и не действовал. Blame бесполезен — важнее что мы изменили в процессе.

### Stakeholders
**Вопрос:** "How did you communicate the failure?"
**Ответ:** Прозрачно. Я сам написал post-mortem и презентовал его команде и менеджменту. Признал свою долю ответственности. Предложил конкретные улучшения.

### Prevention
**Вопрос:** "How do you prevent this now?"
**Ответ:** Три изменения: 1) Customer validation перед началом, 2) Prototype/demo в первые 2 недели, 3) "Clarifying questions" checklist перед стартом.

### Emotions
**Вопрос:** "How did it feel?"
**Ответ:** Frustrating. 2 месяца работы — это больно. Но frustration превратил в action: создал процессы, которые помогают всей команде.

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

**One-liner:** 2 месяца работы на фичу, которую клиент не хотел → взял ownership → создал процесс customer validation.

**Ключевые точки:**
1. "Real-time alerting" оказался "weekly digest" — miscommunication
2. Видел warning signs, но проигнорировал
3. Взял часть ответственности, не blame
4. Создал процесс validation перед началом работы
5. Урок: "Clear requirements" — иллюзия, я должен понять problem

---

## Отличие от Story #3

| Аспект | #3 (Production Incident) | #6 (Wrong Direction) |
|--------|--------------------------|----------------------|
| Тип failure | Technical mistake | Requirements misunderstanding |
| Timeline | Часы | Месяцы |
| Impact | Users affected | Wasted effort |
| Root cause | Shortcut on testing | Not validating assumptions |
| Fix | Technical + process | Process + communication |

**Используй #6 когда:**
- Вопрос про "wrong decision", не "mistake"
- Вопрос про работу с stakeholders
- Уже использовал #3 на предыдущий вопрос
