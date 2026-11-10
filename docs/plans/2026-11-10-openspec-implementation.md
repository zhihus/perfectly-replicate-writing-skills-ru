# План: внедрение интеграции OpenSpec в скилл

> Дата: 2026-11-10. Статус: утверждён. Основано на аудите `docs/openspec-integration-analysis.md` (совместимость 8/10 → цель 10/10).

## Цель
Поднять совместимость скилла `perfectly-replicate-writing-skills-ru` с фреймворком OpenSpec с 8/10 до 10/10, добавив скиллу **режим генерации по готовому контент-спеку** (Mode C), не ломая текущую архитектуру (фазы 0–4, 7+1 измерений, Blind Test, анализ образцов).

## Ограничения
- Не ломать архитектуру: фазы 0–4 и 7+1 измерений остаются.
- Не удалять существующие функции: Blind Test, анализ образцов, Output A/B.
- Изменения только в `SKILL.md` + новый файл `references/openspec-mapping.md`.
- Сначала план (этот файл), затем внедрение.

## Изменения

### 1. Новый режим в `SKILL.md` → Phase 3: Dual Outputs
Добавить подсекцию **`### Mode C — Content-Driven Generation`** после `### Output B — Persona Sub-Skill` (посл. строка «After creating, tell the user it's installed and how to invoke it.»).

Содержание:
- Триггер: пользователь подаёт на вход OpenSpec `spec.md` + Style Card.
- Инструкция: **не анализировать новые образцы** (Phase 0–2 не запускаются — Style Card уже готова). Сразу генерация ТЗ: `spec.md` = источник фактов, Style Card = стилистический фильтр.
- Указатель на `references/openspec-mapping.md` для правил маппинга.
- **Правило языка** (изменение №4): артефакты OpenSpec могут быть на английском → переводить факты на русский, сохраняя технические термины (ТМЦ, СиМ, ТСД) и стилевые маркеры из Style Card.
- Двойная валидация: spec compliance (покрыты ли Requirements/Scenarios) + style compliance (Blind Test ≥70%).

### 2. Новый файл `references/openspec-mapping.md`
Правила маппинга артефактов OpenSpec → структура ТЗ 1С:
- `Requirement` → Раздел ТЗ (например, «Бизнес-логика»).
- `Scenario (WHEN/THEN)` → Строка в таблице тестирования («Действие» / «Ожидаемый результат»).
- `Design.md` (Risks) → Раздел «Ограничения и допущения» в ТЗ.
- `proposal.md` (Why/What) → Введение / цель ТЗ.
- Таблица соответствия + правила «не менять факты, только переводить в формат ТЗ».

### 3. Правила управления контекстом в `SKILL.md` → Phase 0: Ingestion
Добавить подсекцию **`### Content-Driven Input (OpenSpec spec.md)`** после `### Updating Existing Clone`.

Содержание:
- Распознавание входа: `spec.md` + Style Card → это вход Mode C (не анализ образцов).
- **Правило контекста**: если `spec.md` > 4000 символов ИЛИ содержит много Capabilities → разбивать генерацию на этапы: по одной секции/капability за раз, чтобы не потерять Style Card из-за переполнения контекста. Style Card держать в контексте на каждом этапе.

### 4. Правило языка в `SKILL.md`
Включено в Mode C (изменение №1) как отдельное правило. Дополнительно — явное указание: вывод ТЗ на языке автора (русский для 1С), термины не переводятся.

### 5. Обновление `When to Activate` + `Reference Files`
- В `When to Activate` добавить триггер: «generate a ТЗ from an OpenSpec spec.md using a Style Card».
- В `Reference Files` добавить строку: `references/openspec-mapping.md — Mapping rules: OpenSpec artifacts → 1С business-TZ structure`.

## Что НЕ меняется
- Phase 1 (7+1 Dimension Analysis), Phase 2 (Style Portrait Synthesis), Phase 4 (Blind Test) — как есть.
- Output A / Output B — как есть.
- Core Principles, 7+1 framework, persona-template, writing-formulas — как есть.

## Верификация
- `SKILL.md` содержит Mode C, правило контекста, правило языка, обновлённые When to Activate и Reference Files.
- `references/openspec-mapping.md` создан и содержит 3 основных правила маппинга (Requirement→Раздел, Scenario→Таблица, Design.Risks→Ограничения).
- Скилл готов к команде-триггеру: «Сгенерируй ТЗ из спеки X, используя Style Card Y».