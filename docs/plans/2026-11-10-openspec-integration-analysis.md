# План: аналитический документ об интеграции OpenSpec и скилла

> Дата: 2026-11-10. Статус: утверждён пользователем.

## Контекст
Пользователь хочет объединить фреймворк **OpenSpec** (spec-driven development) со скиллом `perfectly-replicate-writing-skills-ru`, чтобы разделить «что писать в ТЗ» (требования) от «как это писать» (стилизация). Результат — аналитический документ (не код, без изменений в файлах скилла) по пути:
`C:\WorkProjects\skills\perfectly-replicate-writing-skills-ru\docs\openspec-integration-analysis.md`

## Что изучено (факты для документа)

**Скилл** (`SKILL.md`, `references/`, `examples/`, `.audit/style-card-raitek.md`):
- 4-фазный workflow: Ingestion → 7+1 Dimension Analysis → Style Portrait Synthesis → Dual Outputs → Blind Test.
- Выводы: Style Card (структурированная Markdown-карточка), переносимый промпт стиля, persona-подскилл.
- Русскоязычная адаптация под бизнес-ТЗ 1С: лемматизация, стоп-слова, маркеры (ЗАКАЗЧИК/ИСПОЛНИТЕЛЬ, УТВЕРЖДАЮ, «Рисунок - N», choppy-фразы 8–15 слов).
- Фаза 4 (Blind Test) генерирует тестовые тексты 500–700 слов → скилл **умеет генерировать** текст в стиле.
- `.audit/style-card-raitek.md` — реальный пример Style Card автора бизнес-ТЗ 1С (ровно целевой use-case).

**OpenSpec** (локальная установка + реальные проекты `Manifest`, `InvestPlan`):
- Структура: `openspec/config.yaml`, `openspec/specs/`, `openspec/changes/<name>/`, `openspec/changes/archive/`.
- Change содержит: `proposal.md` (Why/What/Capabilities/Impact), `specs/<capability>/spec.md` (Spec Delta: ADDED/MODIFIED Requirements + Scenarios в формате WHEN/THEN), `design.md` (Context/Goals/Decisions/Risks/Migration), `tasks.md` (FR N.M, волны, stop-criteria), `.openspec.yaml` (schema/created).
- Workflow: propose → apply → archive. Скиллы: openspec-propose, openspec-apply-change, openspec-archive-change, openspec-explore, openspec-sync-specs, openspec-update-change.
- Ключевые принципы: spec deltas (не полные спеки), механическая валидация (`openspec validate`), лимиты размера задач (6000/4000 символов), управление контекстом «корми исполнителя только текущей задачей, а не всем change».

## Ключевой тезис документа
OpenSpec и Скилл — **ортогональные оси**: OpenSpec = **WHAT** (факты, требования, структура, логика), Скилл = **HOW** (лексика, тон, стилевые маркеры / Style Card). `spec.md` из OpenSpec становится *контент-брифом*, Style Card — *стилевой связкой*. Скиллу нужен новый режим входа: «сгенерировать документ из (контент-спека) + (Style Card)».

## Структура документа (5 разделов, как требует ТЗ)
1. **Резюме** — насколько хорошо OpenSpec сочетается со скиллом (оценка совместимости, главный тезис об ортогональности WHAT/HOW).
2. **Схема интеграции** — пошаговый workflow: пользователь → OpenSpec propose (spec.md) → валидация спека → Скилл (spec + Style Card → ТЗ) → двойная валидация (spec compliance + style compliance) → архивация change / обновление Style Card. **Обязательно: конкретная команда-триггер** (как именно пользователь просит скилл: «Сгенерируй ТЗ из спеки X, используя Style Card Y»).
3. **Анализ интерфейсов** — форматы передачи данных: OpenSpec отдаёт Markdown-файлы (proposal/spec/design/tasks); spec.md с Requirements+Scenarios (WHEN/THEN) маппится на «какие разделы/содержание должно быть в ТЗ»; Style Card — компактная Markdown-карточка; оба используют Markdown → естественный handoff. Отметить, что скиллу нужен новый входной режим (контент-бриф), которого сейчас нет.
4. **Матрица ответственности** — таблица «Кто за что отвечает»: OpenSpec владеет фактами/требованиями/структурой/валидацией спека/версионированием changes; Скилл владеет лексикой/тоном/стилевыми маркерами/валидацией стиля/эволюцией стиля. Отдельно — зона пересечения (структура ТЗ) и правило «не мешать».
5. **Риски и минимизация** — конфликт Markdown-диалектов (OpenSpec-заголовки/WHEN-THEN vs 1С-ТЗ-конвенции), конфликт метаданных (config.yaml vs frontmatter), версионирование (changes/archive vs инкрементальные обновления Style Card), переполнение контекста (обработка спека по секциям), смешение языков (англ. артефакты OpenSpec vs рус. вывод скилла), дрейф стиля при жёсткой структуре спека.

## Ограничения, которые соблюдаются
- Не вношу изменений в файлы скилла (только новый файл в `docs/`).
- Не пишу код интеграции — только аналитика.
- Документ на русском (язык скилла и ТЗ).

## Верификация
- Файл создан по заданному пути, содержит все 5 обязательных разделов.
- Факты в документе соответствуют реальному содержимому `SKILL.md`, `references/`, `.audit/style-card-raitek.md` и реальных OpenSpec-проектов (`Manifest/openspec`).
- Матрица ответственности и схема интеграции покрывают все 5 пунктов ТЗ (интерфейсы, разделение ответственности, контекст, обратная связь, риски).