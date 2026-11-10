# Усиление Фазы 0 (Ingestion) — шаг «Clean» под 1С-корпус

## Context

Аудит (задача «независимый аудитор архитектуры AI-скиллов») показал: внешний скрипт очистки не нужен и вреден — «шум» (титульные листы, блоки ЗАКАЗЧИК/ИСПОЛНИТЕЛЬ, паттерн «Рисунок - N») в бизнес-ТЗ 1С является структурным авторским сигналом (зафиксирован в `.audit/style-card-raitek.md:175, 192` и `references/7-dimensions-framework.md:47`). Принята Альтернатива А: отказ от скрипта, усиление существующего шага 2 «Clean» в Фазе 0.

## Изменение

Один файл: `SKILL.md`, Фаза 0 → Process, шаг 2 (строка 51).

**Было:**
```
2. Clean — strip boilerplate, site headers/footers, publication metadata
```

**Станет:**
```
2. Clean — strip only true noise: page numbers, running headers/footers, file metadata, and OCR garbage from embedded screenshots (UI labels, window chrome, tooltips). **Document-template elements are structural signals, not noise — preserve them:** title page, «ЗАКАЗЧИК/ИСПОЛНИТЕЛЬ» blocks, table of contents, «УТВЕРЖДАЮ» blocks, and figure captions «(Рисунок - N)» (hyphen with spaces — a style marker; never normalize to em dash).
   - **Tables** (glossary, access rights, test order): when extraction breaks a table into a jumbled word stream, reconstruct it as logical «Header: Value» pairs before analysis — table structure is itself a style signal (enumeration discipline, parallelism).
   - **Template ≠ duplicate:** repeated formal blocks (glossary, abbreviation lists, standard clauses) are part of the author's template discipline — keep them. Deduplication (step 1) removes only accidental copy-pasted paragraphs, never formal sections.
```

## Три нюанса, заложенные в текст

1. **Таблицы** — восстановление «Header: Value» пар до анализа; табличная структура — стиль-сигнал (пересечение с Dimension 5 «Parallelism», `references/7-dimensions-framework.md:146`).
2. **Изображения** — OCR-мусор (надписи интерфейса, рамка окна) исключается; «Рисунок - N» защищён: сохраняется + запрет нормализации дефиса в тире (перекликается с `references/7-dimensions-framework.md:47`).
3. **Шаблон ≠ дубликат** — формальные разделы (Глоссарий, список сокращений) исключаются из дедупликации (шаг 1); удаляются только случайные копипаст-абзацы.

## Что НЕ меняется

- Нумерация шагов Фазы 0 (1/3/4 остаются как есть).
- Все остальные файлы скилла.
- Архитектура: 5 фаз, 7+1 измерений, no-dependency (правка промпта, без новых зависимостей).

## Верификация

1. Вывести строки 49–60 `SKILL.md` после правки — проверить целостность нумерации шагов.
2. `git diff SKILL.md` — убедиться, что изменена только строка 51 (шаг 2) и ничего больше.