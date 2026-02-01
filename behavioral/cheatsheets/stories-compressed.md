# 5 историй для Googleyness & Leadership

**Цель:** Выучи каждую историю. Начни с one-liner, потом STAR за 30 сек, потом полная версия.

---

## История #1: Разрешение конфликта

### One-liner (выучи!)
> Разрешил архитектурный конфликт с senior инженером через данные и понимание его underlying concerns → hybrid решение.

### STAR за 30 секунд

| S | Проектировали data pipeline. Senior хочет монолит, я — микросервисы. |
|---|----------------------------------------------------------------------|
| T | Архитектура для 10x роста. Нужно убедить или уступить. |
| A | Собрал данные → 1-on-1 понять concerns → предложил hybrid → формальный review |
| R | Hybrid принят, система выдержала 15x, подход стал стандартом |

### Ключевые цифры
- **15x** рост нагрузки выдержала система
- **p99 < 100ms** latency сохранился
- Senior стал **advocate** в следующих проектах

### Черты: Humility, Doing Right, High Standards, Collaboration
### Сигналы: Influence, Driving Results, Decision Making

### Follow-up ответы

**"What alternatives?"**
> Полный монолит, full microservices, hybrid. Для каждого оценивал complexity, scalability, operational overhead.

**"What was hardest?"**
> Держать эго в стороне. Соблазн доказать правоту, но это разрушило бы отношения.

**"Do differently?"**
> Раньше 1-on-1. К моменту review позиции уже публичные — сложнее compromise.

### Урок
> Технические споры — это о underlying concerns, не о технике.

---

## История #2: Лидерство без авторитета

### One-liner (выучи!)
> Инициировал frontend миграцию без позиции: PoC → данные → stakeholder buy-in → координировал 3 команды → 70% за 4 месяца.

### STAR за 30 секунд

| S | Legacy React (class + redux-saga). Никто не владеет модернизацией. |
|---|---------------------------------------------------------------------|
| T | Сам определил проблему, сам взял инициативу. Нет выделенного времени. |
| A | PoC (2 вечера) → собрал данные о pain points → 1-on-1 с tech leads → RFC → координировал execution |
| R | 70% мигрировано, time-to-feature -35%, приглашён в архитектурный комитет |

### Ключевые цифры
- **70%** codebase за **4 месяца**
- **-35%** time-to-feature
- **-50%** bug rate в новом коде
- **+40%** developer satisfaction

### Черты: Ambiguity, Bias for Action, Ownership, High Standards, Innovation, Collaboration
### Сигналы: Initiative, Influence, Driving Results, Decision Making

### Follow-up ответы

**"How did you get buy-in?"**
> Индивидуальные 1-on-1 с каждым tech lead, PoC с измеримыми результатами, готовность лично помогать.

**"What was hardest?"**
> Получить время от команд без формальной власти. Решил через демонстрацию value.

**"Do differently?"**
> Formal metrics dashboard с самого начала для tracking progress.

### Урок
> Leadership — это ownership, не позиция. Видишь проблему — реши её.

---

## История #3: Неудача и уроки

### One-liner (выучи!)
> Production incident из-за недостаточного тестирования миграции БД → взял ownership → быстро исправил → создал процессы предотвращения.

### STAR за 30 секунд

| S | Миграция MySQL→PostgreSQL. Дедлайн давит, 50M записей, 24/7 трафик. |
|---|--------------------------------------------------------------------|
| T | Zero-downtime миграция с data integrity. |
| A | Сократил тестирование (10% sample) → encoding issue в production → остановил → fix за 2 часа → full validation → успешно завершил |
| R | 4 часа degraded для 5% users, 0 потерянных данных, новые процессы для migrations |

### Ключевые цифры
- **4 часа** degraded experience
- **5%** users affected (Asia-Pacific)
- **0** data loss
- **Новый процесс**: mandatory full-data testing

### Черты: Humility, Doing Right, Ownership, High Standards
### Сигналы: Decision Making

### Follow-up ответы

**"How did it feel?"**
> Ужасно. Первая реакция — паника. Но transparency — единственный путь.

**"Did you blame the deadline?"**
> Нет. Дедлайн был tight, но решение сократить тестирование — моё. Я мог эскалировать.

**"How prevent now?"**
> 1) Checklist для migrations, 2) Mandatory full-data testing, 3) Явная risk коммуникация.

### Урок
> Shortcuts под давлением → эскалируй риски, не молчи.

---

## История #4: Решение в неопределённости

### One-liner (выучи!)
> Запустил B2B MVP за 6 недель при неясных требованиях: классифицировал решения, построил flexible architecture, короткие feedback loops.

### STAR за 30 секунд

| S | Новый B2B продукт. "Нужен dashboard, но не знаем какие метрики". 6 недель. 1 инженер. |
|---|--------------------------------------------------------------------------------------|
| T | MVP для валидации с клиентами. Нельзя потратить 6 недель на wrong thing. |
| A | Mapped uncertainties → reversible vs irreversible decisions → flexible architecture first → weekly client demos с недели 3 |
| R | MVP готов, 3 paid pilots, 30% features pivot (validated by clients) |

### Ключевые цифры
- **5.5 недель** — MVP готов
- **3 клиента** → paid pilot
- **30%** features pivot (правильное решение!)
- B2B profitable через **8 месяцев**

### Черты: Ambiguity, Bias for Action, Ownership, Innovation
### Сигналы: Initiative, Driving Results, Decision Making

### Follow-up ответы

**"How decide what to build first?"**
> Infrastructure over features. Фичи изменятся (high uncertainty), infrastructure останется (low uncertainty).

**"What if built wrong thing?"**
> Weekly demos = max 1 неделя wasted work. Без фидбека риск намного выше.

**"Could wait for clearer requirements?"**
> Дедлайн фиксирован (partnership). Вопрос — как снизить risk, не избежать uncertainty.

### Урок
> При uncertainty — инвестируй в flexibility, не в угадывание. Короткие feedback loops.

---

## История #5: Главное достижение

### One-liner (выучи!)
> Инициировал cloud cost optimization: quick wins в своё время → показал value → получил sprint → $200K/год savings.

### STAR за 30 секунд

| S | Cloud costs растут 20%/квартал → $800K/год. Обсуждают, но нет плана. |
|---|----------------------------------------------------------------------|
| T | Не моя задача. Но увидел проблему, которую мог решить. |
| A | Анализ → quick wins (unused, oversized) за 2 недели → $65K → показал менеджеру → получил sprint → deep optimization (Spark, Spot instances) |
| R | -$200K/год (25%), latency -15%, cost awareness стала culture, повышение через 6 мес |

### Ключевые цифры
- **$200K/год** savings (**25%** reduction)
- **$65K** за первые 2 недели part-time
- **-15%** latency (side effect)
- ROI: **4 недели работы** → $200K/год

### Черты: Bias for Action, Ownership, High Standards, Innovation
### Сигналы: Initiative, Influence, Driving Results, Decision Making

### Follow-up ответы

**"Why if not your job?"**
> 1) Мог это сделать (cloud опыт), 2) Очевидная проблема без owner, 3) Хотел показать impact beyond текущих задач.

**"What if broke something?"**
> Начинал с low-risk (unused resources). Staged rollout с monitoring для серьёзных changes.

**"How ensure costs don't creep back?"**
> Dashboard с alerts, quarterly cost review в rituals, cost estimation в design docs.

### Урок
> Большие проблемы решай через маленькие доказуемые шаги. Start small, prove value, scale.

---

## История #6: Неудача (Backup)

### One-liner (выучи!)
> 2 месяца работы на фичу, которую клиент не хотел → взял ownership → создал процесс customer validation.

### STAR за 30 секунд

| S | Клиент попросил "real-time alerting". PM интерпретировал, начали разработку. |
|---|-----------------------------------------------------------------------------|
| T | Спроектировать и реализовать alerting систему за 3 месяца. |
| A | 2.5 месяца разработки → demo клиенту → "Нам нужен weekly digest, не real-time" → pivot, сохранил 30% работы |
| R | ~70% работы потеряно, но создал процесс customer validation |

### Ключевые цифры
- **2 месяца** работы потеряно
- **30%** кода переиспользовано
- **3 недели** опоздание на delivery
- Новый процесс предотвратил **2+** похожие ситуации

### Черты: Humility, Doing Right, Ownership, Collaboration
### Сигналы: Decision Making

### Follow-up ответы

**"Was this the PM's fault?"**
> PM ошибся, но я тоже. Видел warning signs, не действовал. Blame бесполезен — важнее что изменили.

**"How do you prevent this now?"**
> Customer validation перед началом, prototype в первые 2 недели, checklist вопросов.

### Урок
> "Clear requirements" — иллюзия. Я отвечаю за понимание problem, не просто execution task.

### Когда использовать вместо #3
- Вопрос про "wrong direction", не "technical mistake"
- Вопрос про работу с stakeholders
- Уже использовал #3 на предыдущий вопрос

---

## Матрица покрытия

### Истории → Черты Googleyness

| История | Amb | Hum | Bias | Right | Own | High | Inn | Coll |
|---------|-----|-----|------|-------|-----|------|-----|------|
| #1 Conflict | | + | | + | | + | | + |
| #2 Leadership | + | | + | | + | + | + | + |
| #3 Failure | | + | | + | + | + | | |
| #4 Ambiguity | + | | + | | + | | + | |
| #5 Achievement | | | + | | + | + | + | |
| #6 Failure (backup) | | + | | + | + | | | + |

### Истории → Leadership Сигналы

| История | Init | Inf | Ment | Res | Dec |
|---------|------|-----|------|-----|-----|
| #1 Conflict | | + | | + | + |
| #2 Leadership | + | + | **+** | + | + |
| #3 Failure | | | | | + |
| #4 Ambiguity | + | | | + | + |
| #5 Achievement | + | + | | + | + |
| #6 Failure (backup) | | | | | + |

> **Обрати внимание:** #2 теперь покрывает **Mentorship** (обучение команд при миграции)

---

## Quick Test: Вспомни за 10 секунд

1. **Conflict:** Hybrid через ___ и ___
2. **Leadership:** ___ миграция, ___% за 4 мес + ___
3. **Failure:** ___ incident → ___ → новые процессы
4. **Ambiguity:** MVP + ___ architecture + weekly ___
5. **Achievement:** Cloud costs -$___K через quick wins
6. **Failure backup:** ___месяца на wrong feature → ___ validation

<details>
<summary>Ответы</summary>

1. данные, понимание concerns
2. Frontend, 70, обучил команды
3. Production, ownership
4. flexible, client demos
5. 200
6. 2, customer

</details>
