# OpenSpec → 1С Business-ТЗ Mapping Rules

Reference for **Mode C (Content-Driven Generation)**: how to translate OpenSpec artifacts into the structure of a 1С business technical specification (ТЗ).

**Core principle:** the OpenSpec `spec.md` is the **source of facts** (WHAT). The Style Card is the **stylistic filter** (HOW). This file defines only the **structural mapping** — it never changes, invents, or drops facts. You *translate* the spec into the ТЗ format and *dress* it in the author's style; you do not re-think the logic.

---

## Artifact Roles

| OpenSpec artifact | Role in Mode C | Used? |
|-------------------|----------------|-------|
| `specs/<capability>/spec.md` | **Content brief (primary input)** — Requirements + Scenarios | ✅ Always |
| `proposal.md` | Context — Why / What Changes → introduction / purpose | ✅ Optional |
| `design.md` | Risks / Trade-offs / Decisions → «Ограничения и допущения» | ✅ Risks only |
| `tasks.md` | Implementation plan — **not** part of the ТЗ content | ❌ No |
| `.openspec.yaml`, `config.yaml` | Framework metadata — **not** content | ❌ No |

---

## Mapping Table

| OpenSpec element | ТЗ element | Notes |
|------------------|------------|-------|
| `## Purpose` (spec.md) | Введение / Цель ТЗ | Translate to Russian; keep scope intact |
| `### Requirement: <name>` | Раздел ТЗ (e.g. «Бизнес-логика», «Функциональные требования») | One Requirement → one numbered ТЗ section |
| `#### Scenario: <name>` + `WHEN` / `THEN` | Декларативная проза (по умолчанию) | `WHEN` → «Действие», `THEN` → «Ожидаемый результат»; таблица — только если Style Card или конкретный раздел (напр. «Права доступа») явно требует |
| `MUST` / `SHALL` (in Requirement body) | Обязательное требование | Render as a firm requirement (bold / «Требование:») |
| `design.md` → `## Risks / Trade-offs` | Раздел «Ограничения и допущения» | Translate each risk; do not add new ones |
| `proposal.md` → `## Why` | Обоснование / Актуальность | Optional, for the introduction |
| `proposal.md` → `## What Changes` | Перечень изменений / Задачи | Optional |

---

## The Three Core Mappings (required)

### 1. `Requirement` → Раздел ТЗ
Each `### Requirement:` becomes a numbered section of the ТЗ. The requirement body (the `MUST`/`SHALL` statements) becomes the section's functional description.

- Keep the requirement **complete** — do not summarize away a `MUST`.
- Section title: derive from the requirement name, in Russian, in the author's register (see Style Card).
- Numbering follows the 1С ТЗ convention (`1.`, `1.1.`, `1.1.1.`), **not** the OpenSpec heading levels.

### 2. `Scenario (WHEN/THEN)` → Декларативная проза (по умолчанию)
Each `#### Scenario:` becomes **declarative prose** in the ТЗ testing section (e.g. §3.4 «Порядок тестирования и приемки») — the author's default rendering. A `WHEN` clause becomes the «Действие», a `THEN` clause becomes the «Ожидаемый результат», written as consecutive declarative sentences.

- **Prose by default.** Render scenarios as flowing declarative sentences, not a table. This matches the author's real ТЗ (section 3.4 is prose, not a table).
- **Table only when required.** Use a «Действие / Ожидаемый результат» table **only** if the Style Card or a specific section explicitly requires it (e.g. «Права доступа к функционалу», where the author does use a table):

  | Действие (WHEN) | Ожидаемый результат (THEN) |
  |-----------------|----------------------------|
  | <translated WHEN clause> | <translated THEN clause> |

- One scenario → one or more consecutive sentences (split into multiple sentences only if it has multiple distinct WHEN/THEN pairs).
- Preserve the **exact** trigger and expected outcome — translate, don't re-interpret.
- Use the author's standard constructions from the Style Card (e.g. «По нажатию на кнопку X …», «Система …») where they fit.
- **No telegraphic style.** Do not fragment the prose into 4–5 word sentences; merge short adjacent sentences into compound sentences of 8–15 words (see SKILL.md → Phase 3 Mode C, «No choppy robot» rule).

### 3. `Design.md` (Risks) → Раздел «Ограничения и допущения»
The `## Risks / Trade-offs` section of `design.md` becomes the ТЗ section «Ограничения и допущения».

- Translate each risk/trade-off into a limitation or assumption.
- Do **not** invent limitations that are not in `design.md`.
- Do **not** copy implementation decisions (e.g. "use bash") into the ТЗ — those belong to `design.md` / `tasks.md`, not to the business ТЗ.

---

## Language Rule (CRITICAL)

OpenSpec artifacts are often written in **English**. When generating the ТЗ:

1. **Translate facts into Russian** (the author's language for a 1С business-ТЗ).
2. **Preserve technical terms unchanged** — do not translate abbreviations or domain terms: `ТМЦ`, `СиМ`, `ТСД`, `АРМ`, `УЛ`, `ФР`, `ПВХ`, `FEFO`, `ZPL`, and any product/configuration names (e.g. `1С:ERP`).
3. **Apply the Style Card** — lexicon, tone, punctuation fingerprint («Рисунок - N» with hyphen, «ЗАКАЗЧИК/ИСПОЛНИТЕЛЬ», «УТВЕРЖДАЮ»), choppy phrases (8–15 words), disallowed list.
4. **Never re-interpret logic** — translate the WHEN/THEN and the `MUST` statements faithfully; the OpenSpec spec is the single source of truth for content.

---

## Section-by-Section Generation (large specs)

If the `spec.md` exceeds **4,000 characters** or contains **multiple Capabilities**:

1. Split by **Capability** (or by `### Requirement:` group).
2. Generate the ТЗ **one section at a time**, keeping the Style Card in context on every pass (it is compact and always fits).
3. Assemble the sections into a single ТЗ, preserving the author's structural markers (numbering, «УТВЕРЖДАЮ», оглавление, «Рисунок - N»).

---

## Dual Validation (gate before finalizing)

- **(A) Spec compliance** — every `Requirement` and `Scenario` from `spec.md` is covered by a ТЗ section / table row. Nothing added, nothing dropped.
- **(B) Style compliance** — run the Phase 4 Blind Test on a generated fragment; composite match **≥70%** against the source corpus.

If either gate fails, flag the discrepancy and regenerate the affected section.