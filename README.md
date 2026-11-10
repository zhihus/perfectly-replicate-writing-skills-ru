# Perfect Replicate Distillation Writing Skill

> **Clone any author's writing style** by deeply analyzing their content across 7+1 dimensions.

An open-source rebuild of the writing-style cloning skill spec published by JinDaGe on jindage.com.

## Overview

This skill takes 3+ documents by an author and, through a 4-phase deep analysis, produces two things:

- **Output A: Portable Writing Style Prompt** — a self-contained style instruction usable with any LLM (Claude, ChatGPT, etc.)
- **Output B: Persona Sub-Skill** — an installable persona writing skill for Claude Code

The skill's instructions are written in English (token-efficient), but the **output** (style card, persona, style prompt) is generated **in the author's language**. For a Russian-speaking author, all output is in Russian.

## Analysis Framework: 7+1 Dimensions

| Dimension | Measured | Analysis Mode |
|-----------|----------|---------------|
| 1. Lexical | word frequency, fingerprint words, collocations, lexical richness | parallel |
| 2. Syntactic | avg sentence length, clause-type ratios, punctuation fingerprint, paragraph rhythm | parallel |
| 3. Tonal | colloquialism index, sentence-length volatility, formality, certainty | parallel |
| 4. Discourse | opening/closing patterns, paragraph rhythm, logic flow | sequential |
| 5. Rhetoric | metaphor density, parallelism, rhetorical questions, concrete/abstract ratio | parallel |
| 6. Emotion | polarity distribution, emotional arc, stance markers | parallel |
| 7. Perspective | theme clusters, pronoun distribution, imagery system, value words | parallel |
| 8. Format Adaptation | structural inertia, genre fit, formula recommendation | last |

## 4-Phase Workflow

### Phase 0: Ingestion
- Accept ≥3 documents (≥2,000 total words)
- Ideal: ≥10 documents / ≥10,000 total words
- Automatic deduplication, cleaning, statistics
- Count volume in **words** for Russian and English; in **characters** only for Chinese

### Phase 1: 7+1 Dimension Analysis
- Systematic measurement of the 7 dimensions (mixed parallel + sequential)
- Format-adaptation assessment last

### Phase 2: Style Portrait Synthesis
- Distill 3-5 core conclusions per dimension
- Cross-dimension validation to extract 3-5 "writing genes"
- Generate a 5-10 item disallowed list

### Phase 3: Dual Outputs
- **Output A**: portable style prompt
- **Output B**: persona sub-skill

### Phase 4: Blind Test
- Generate 2 test pieces of **500-700 words** (≈3,500-5,000 chars for Russian)
- Score each against the original across all 7 dimensions
- Composite match >70% passes (evaluated on the full 500-700 word volume)

## Project Structure

```
perfectly-replicate-writing-skills-ru/
├── SKILL.md                              # Main skill definition (Claude Code SKILL.md)
├── README.md                             # This file
├── LICENSE                               # MIT License
├── references/
│   ├── 7-dimensions-framework.md         # 7+1 dimension analysis framework details
│   ├── writing-formulas.md              # 14 writing formulas reference
│   └── persona-template.md              # Persona sub-skill generation template
└── examples/
    └── 抓马教育-style/                    # Example output: 抓马教育 brand style analysis
        ├── SKILL.md                      # Generated persona sub-skill
        └── style-card.md                 # Full 7+1 dimension style card
```

## Usage

### Install as a Claude Code skill

```bash
# Clone the project
git clone https://github.com/zrh091110225/perfectly-replicate-writing-skills.git
cd perfectly-replicate-writing-skills-ru

# Install the skill
cp -r SKILL.md ~/.claude/skills/perfectly-replicate-writing-skills-ru/
cp -r references ~/.claude/skills/perfectly-replicate-writing-skills-ru/
```

Then trigger in Claude Code:

> "Analyze the author's style from these articles"
> "Clone this writer's writing style"
> "Write an article about efficiency in the style of [author]"

### Use as a standalone tool

Manually run the 4-phase workflow:
1. Collect ≥3 documents by the target author
2. Analyze across the 7+1 dimensions
3. Synthesize the style portrait
4. Generate the style prompt and persona sub-skill

## Core Principles

1. **Features appearing 3+ times are real style signals** — once is improvisation, three is style
2. **Preserve contradictions** — don't force-smooth inconsistent authorship
3. **Be specific** — "average sentence length 12 words, 30% of sentences under 8 words" is useful; "likes short sentences" is not
4. **The disallowed list matters more than positive features** — defining style boundaries by "what the author never does" often works better
5. **No guessing** — don't invent author background; don't pass general writing advice off as unique style
6. **Don't smooth imperfections** — awkward phrasings, quirks, and rough edges are identity markers

## License

MIT License

## Credits

- Framework design inspired by [JinDaGe](https://jindage.com)'s [Perfectly Replicate Writing Skills](https://jindage.com/en/skills/perfectly-replicate-writing-skills) on the skill marketplace
- This version is rebuilt from the public spec description; original rights belong to the author