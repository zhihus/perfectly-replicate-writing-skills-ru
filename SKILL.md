---
name: perfectly-replicate-writing-skills-ru
description: Clone any author's writing style by analyzing 3+ articles across 7+1 dimensions (lexical, syntactic, tonal, discourse, rhetoric, emotion, perspective + format adaptation). Outputs a portable style prompt and an installable persona sub-skill. Use when the user wants to: (1) clone an author's voice from writing samples, (2) generate content that precisely matches a specific writer's style, (3) build a reusable writing persona for ongoing content creation, or (4) analyze and document writing style characteristics systematically.
---

# Perfect Replicate Distillation Writing Skill

Clone any author's writing style by deeply analyzing their content across 7+1 dimensions. Output: (A) a portable Writing Style Prompt usable with any LLM, (B) an installable persona sub-skill for ongoing use.

## When to Activate

Trigger on requests for:
- "clone this writer's style" / "write in the style of [author]"
- style analysis of provided articles or content
- building a reusable writing persona from samples
- "make my writing sound more like [person]"

## Core Principles

1. **Features appearing 3+ times in separate articles are real style signals** — once-offs are improvisation, not patterns.
2. **Preserve contradictions** — when the author writes inconsistently across articles, don't smooth it away. Contradictions are valuable.
3. **Be specific, not vague** — "средняя длина предложения 12 слов, 30% предложений короче 8 слов" is useful; "любит короткие предложения" is not.
4. **Negatives define boundaries** — what the author *never* does often defines style better than what they do.
5. **No guessing** — never invent author background, biography, or intent. Never pass general writing advice as unique stylistic insight.
6. **Don't smooth imperfections** — awkward phrasings, quirks, and rough edges are identity markers. Preserve them.

## Workflow Overview

This skill runs in **4 phases**. Execute them sequentially:

- **Phase 0: Ingestion** — Collect, deduplicate, and validate source material
- **Phase 1: 7+1 Dimension Analysis** — Analyze each dimension of writing style
- **Phase 2: Style Portrait Synthesis** — Cross-validate, extract writing genes, build style card
- **Phase 3: Dual Outputs** — Generate (A) portable style prompt + (B) installable persona sub-skill
- **Phase 4: Blind Test** — Generate test pieces and score accuracy

---

## Phase 0: Ingestion

Accept input via: direct paste, file paths, or conversation context.

### Requirements
- **Minimum**: 3 documents, ≥2,000 total words
- **Ideal**: ≥10 documents, ≥10,000 total words
- Detect language automatically (Russian / English / Chinese / mixed)
- Count volume in **words** for Russian and English, in **characters** only for Chinese

### Process
1. Deduplicate — remove near-duplicate content (cosine similarity >0.85 on first 200 chars)
2. Clean — strip only true noise: page numbers, running headers/footers, file metadata, and OCR garbage from embedded screenshots (UI labels, window chrome, tooltips). **Document-template elements are structural signals, not noise — preserve them:** title page, «ЗАКАЗЧИК/ИСПОЛНИТЕЛЬ» blocks, table of contents, «УТВЕРЖДАЮ» blocks, and figure captions «(Рисунок - N)» (hyphen with spaces — a style marker; never normalize to em dash).
   - **Tables** (glossary, access rights, test order): when extraction breaks a table into a jumbled word stream, reconstruct it as logical «Header: Value» pairs before analysis — table structure is itself a style signal (enumeration discipline, parallelism).
   - **Template ≠ duplicate:** repeated formal blocks (glossary, abbreviation lists, standard clauses) are part of the author's template discipline — keep them. Deduplication (step 1) removes only accidental copy-pasted paragraphs, never formal sections.
3. Count corpus stats — N articles, total chars/words, per-article breakdown, avg length
4. Collect metadata — ask user for: author name, platform, genre (if not provided)

### Quality Assessment
- **Insufficient** (<3 documents / <2,000 words): warn user, proceed with "incomplete" flag
- **Mixed types**: separate by genre, ask user which to distill
- **Suspected multiple authors**: flag inconsistencies, warn about team-written accounts
- **Self-distillation**: user's own articles — frame identity as "you are yourself," surface unconscious habits

### Updating Existing Clone
If a persona sub-skill already exists for this author, read the existing `SKILL.md`, do a delta-analysis on new articles only, and incrementally update rather than rebuilding from scratch.

---

## Phase 1: 7+1 Dimension Analysis

Load and follow the detailed framework in `references/7-dimensions-framework.md`. The 7 dimensions plus format adaptation:

### Group A — Basic Stats (run in parallel)
1. **Lexical** — Top-30 word frequency, 10-15 "fingerprint words" (unusually frequent vs. general corpus), collocations (2-3 word pairs), neologism/archaism detection
2. **Syntactic** — Avg sentence length, clause type ratios (simple/compound/complex), punctuation patterns (dash/colon/semicolon frequency), paragraph length distribution
3. **Tonal** — Colloquialism index, sentence-length volatility (stddev / mean), lexical diversity (TTR or MATTR)

### Group B — Structure (run sequentially)
4. **Discourse** — Opening types (anecdote/data/question/declaration), closing patterns, paragraph length rhythm (short-long-short patterns), logic flow (deductive/inductive/causal)

### Group C — Semantic (run in parallel after B)
5. **Rhetoric & Expression** — Metaphor/simile frequency, parallelism usage, rhetorical questions, concrete vs. abstract ratio
6. **Emotion & Attitude** — Polarity distribution (positive/negative/neutral), emotional arc across articles, stance markers (hedging/certainty)
7. **Content Perspective** — Theme clusters, imagery patterns, pronoun density (1st/2nd/3rd person), value-word clusters

### Dimension 8 — Format Adaptation (after all 7 above)
- Structural inertia score (how rigidly the author follows genre conventions)
- Capability matching — which formats/genres the author's style naturally fits
- Formula recommendation — 4-point scale per writing formula: "born fit" / "can handle" / "poor fit" / "clashes"

### Output
Structured analysis with quantified observations for each dimension. Reference `references/7-dimensions-framework.md` for measurement techniques and thresholds.

---

## Phase 2: Style Portrait Synthesis

### Distillation
Distill each of the 7 dimensions into **3-5 core conclusions** each. Prefer quantified statements over vague impressions.

### Cross-Validation: Extract "Writing Genes"
Find **3-5 writing genes** — features that:
- Are supported by ≥2 independent dimensions
- Are distinctive to this author (not general good writing advice)
- Would make the text feel wrong if removed

### Disallowed List
Generate a **5-10 item disallowed list** — specific patterns, tics, moves, or word choices the author never uses. These often define style boundaries better than positive features.

### Output: Author Writing Style Card
A structured Markdown document containing:
- Author metadata (name, platform, genre, corpus stats)
- Per-dimension findings (3-5 points each)
- Format adaptation recommendations
- Writing genes (3-5 items)
- Disallowed list (5-10 items)

---

## Phase 3: Dual Outputs

### Output A — Writing Style Prompt
A self-contained prompt usable with any LLM (Claude, ChatGPT, etc.), containing:

1. **Identity statement** — 100-200 chars (Russian) / 15-25 words, written in the author's voice, describing who they are
2. **Writing Iron Laws** — 5-8 non-negotiable rules the LLM must follow
3. **Style Reference Card** — concise per-dimension guidance with specific examples
4. **Format Adaptation** — which formats/genres this style works for
5. **Mode Selection** — recommended writing formulas from the 14 available (see `references/writing-formulas.md`)
6. **Disallowed List** — what to never do
7. **Pre-Writing Mantra** — a short prompt-prefix that primes the style

### Output B — Persona Sub-Skill
An installable Claude Code skill at `~/.claude/skills/[author-name]-style/SKILL.md`:

- Read `references/persona-template.md` for the skeleton
- Fill all template slots with the style portrait
- Name: `[author-name]-style`
- Description: "Write in the style of [author name], a [genre] writer on [platform]"
- Include the full style reference card, iron laws, disallowed list, and mode selection
- Support all 14 writing formulas (see `references/writing-formulas.md`)

After creating, tell the user it's installed and how to invoke it.

---

## Phase 4: Blind Test

Generate two **500-700 word** test pieces (≈3,500-5,000 chars for Russian):
1. **Default mode** — write naturally in the cloned style
2. **Formula mode** — write using one of the recommended formulas

Compare each against a randomly-selected **500-700 word** excerpt from the source corpus (unseen during analysis). The longer volume is required for Russian so the test can demonstrate paragraph structure, logical connectives, and syntax — not just vocabulary.

### Match Analysis
Score each dimension as a star rating (★☆☆☆☆ to ★★★★★), producing a composite match percentage.

Include specific evidence: "The test piece uses 'X' (present in original: 12 times across 8 articles) but lacks 'Y' (present in original: 7 times) and introduces 'Z' (0 times in original)."

### Quality Gate
If composite score <70% for either test piece, flag discrepancies and offer to re-run a refined version. The 70% threshold is evaluated on the full 500-700 word volume (not on a short excerpt).

---

## Special Scenarios

### Too Little Material
If <3 documents or <2,000 words: warn user, proceed with "incomplete" flag. All outputs flagged as provisional.

### Mixed Article Types
If a single author writes across genres (e.g., technical tutorials + personal essays): separate style by genre, ask user which to distill (or all).

### Suspected Multiple Authors
If writing style is inconsistent (e.g., some articles use heavy metaphor, others are purely factual): flag for user review, warn about team-written accounts.

### Updating Existing Clone
Read existing `SKILL.md`, delta-analysis on new articles only, incremental update.

### Self-Distillation
User's own writing. Frame identity as "you are the author" — help surface unconscious habits the user may not notice.

---

## Reference Files

- `references/7-dimensions-framework.md` — Detailed measurement framework for all 7+1 dimensions
- `references/writing-formulas.md` — 14 writing formulas with descriptions and when to use each
- `references/persona-template.md` — Template for generating the persona sub-skill
