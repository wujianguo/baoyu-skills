---
name: baoyu-slide-deck
description: Generates Slidev presentations from content. Creates an outline then a complete Slidev markdown file (slides.md) ready to run in the browser or export to PDF/PPTX via Slidev. Use when user asks to "create slides", "make a presentation", "generate deck", "slide deck", "PPT", or "slidev".
version: 1.56.2
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-slide-deck
---

# Slide Deck Generator (Slidev)

Transform content into a [Slidev](https://sli.dev) presentation — a web-based, Markdown-driven slide deck that runs in the browser and can be exported to PDF or PPTX.

## User Input Tools

When this skill prompts the user, follow this tool-selection rule (priority order):

1. **Prefer built-in user-input tools** exposed by the current agent runtime — e.g., `AskUserQuestion`, `request_user_input`, `clarify`, `ask_user`, or any equivalent.
2. **Fallback**: if no such tool exists, emit a numbered plain-text message and ask the user to reply with the chosen number/answer for each question.
3. **Batching**: if the tool supports multiple questions per call, combine all applicable questions into a single call; if only single-question, ask them one at a time in priority order.

Concrete `AskUserQuestion` references below are examples — substitute the local equivalent in other runtimes.

## Confirmation Policy

Default behavior: **confirm before generation**.

- Treat explicit skill invocation, a file path, matched signals/presets, and `EXTEND.md` defaults as **recommendation inputs only**. None of them authorizes skipping confirmation.
- Do **not** start Step 3 or later until the user completes Step 2.
- Skip confirmation only when the current request explicitly says to do so, for example: "直接生成", "不用确认", "跳过确认", "按默认出幻灯片", or equivalent wording.
- If confirmation is skipped explicitly, state the assumed theme / audience / slide-count / language in the next user-facing update before generating.

## Language

Respond in the user's language across questions, progress reports, error messages, and the completion summary. Keep technical tokens (style names, file paths, code) in English.

## Options

| Option | Description |
|--------|-------------|
| `--theme <name>` | Slidev theme name (see Theme System below) |
| `--audience <type>` | beginners / intermediate / experts / executives / general |
| `--lang <code>` | Output language (en, zh, ja, ...) |
| `--slides <N>` | Target slide count (8-25 recommended, max 30) |
| `--outline-only` | Stop after outline |

## Theme System

Slidev themes control the overall visual design. Select by matching content signals.

### Themes

| Theme | Best For |
|-------|----------|
| `default` (Default) | General purpose, clean and minimal |
| `seriph` | Academic, research, professional |
| `apple-basic` | Executive briefings, product decks |
| `bricks` | Product launches, marketing, keynotes |
| `penguin` | Creative, startup, vibrant talks |
| `eloc` | Developer talks, code walkthroughs |
| `purplin` | Creative, lifestyle, design topics |
| `geist` | Tech product, SaaS, minimal decks |
| `the-unnamed` | Editorial, magazine-style |
| `hep` | Scientific, physics, academic |

### Auto-Selection

Match content signals to a theme. Pick the first row whose signal keywords appear in the source; fall back to `default` if nothing matches.

| Signals in source | Theme |
|-------------------|-------|
| tutorial, learn, education, guide, beginner | `seriph` |
| classroom, teaching, school, academic | `seriph` |
| architecture, system, data, analysis, technical | `default` |
| executive, minimal, clean, simple | `apple-basic` |
| saas, product, dashboard, metrics | `geist` |
| investor, quarterly, business, corporate | `apple-basic` |
| launch, marketing, keynote, magazine | `bricks` |
| entertainment, music, gaming | `purplin` |
| explainer, journalism, science communication | `the-unnamed` |
| gaming, retro, developer, code | `eloc` |
| biology, chemistry, medical, scientific, physics | `hep` |
| lifestyle, wellness, travel, creative | `penguin` |

### Slide Count Heuristic

| Source length | Recommended slides |
|---------------|--------------------|
| < 1000 words | 5-10 |
| 1000-3000 words | 10-18 |
| 3000-5000 words | 15-25 |
| > 5000 words | 20-30 (consider splitting) |

## File Layout

```
slide-deck/{topic-slug}/
├── outline.md
└── slides.md
```

**Slug**: 2-4 words, kebab-case, extracted from topic. "Introduction to Machine Learning" → `intro-machine-learning`.

**Backup rule**: if a file about to be written already exists, rename it to `<name>-backup-YYYYMMDD-HHMMSS.<ext>` before writing the new one.

## Workflow

Copy this checklist and check off items as you complete them:

```
- [ ] Step 1: Setup & analyze
- [ ] Step 2: Confirmation ⚠️ REQUIRED
- [ ] Step 3: Generate outline
- [ ] Step 4: Review outline (conditional)
- [ ] Step 5: Generate Slidev markdown
- [ ] Step 6: Output summary
```

### Step 1: Setup & Analyze

**1.1 Load EXTEND.md** — check these paths in order; first hit wins:

| Path | Scope |
|------|-------|
| `.baoyu-skills/baoyu-slide-deck/EXTEND.md` | Project |
| `${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-slide-deck/EXTEND.md` | XDG |
| `$HOME/.baoyu-skills/baoyu-slide-deck/EXTEND.md` | User home |

If found, read, parse, and print a summary (theme / audience / language / review). If not, proceed with defaults. Schema: `references/config/preferences-schema.md`.

**1.2 Analyze content** — follow `references/analysis-framework.md`: classify content, detect language, note signals for theme selection, estimate slide count from length (see **Slide Count Heuristic** above), generate topic slug. Save source as `source.md` (honor backup rule if one exists).

**1.3 Check existing output** ⚠️ REQUIRED before Step 2. If `slide-deck/{topic-slug}/` exists, ask how to proceed — four options (regenerate outline / regenerate slides / backup and regenerate / exit), verbatim copy in `references/confirmation.md`.

Save findings to `analysis.md`: topic, audience, signals, recommended theme and slide count, language detection.

### Step 2: Confirmation ⚠️ REQUIRED

**Hard gate**: this step is mandatory per the [Confirmation Policy](#confirmation-policy) — Steps 3+ cannot start until the user confirms here (or explicitly opts out with "直接生成" / equivalent wording in the current request).

Batch four questions in one `AskUserQuestion` call: theme, audience, slide count, review-outline?. Verbatim options in `references/confirmation.md`.

Summary displayed before the questions:
- Content type + topic
- Detected language
- Recommended theme (based on signals)
- Recommended slide count (based on length)

**After confirmation**: update `analysis.md` with final choices and store `skip_outline_review` flag from Q4.

### Step 3: Generate Outline

Build the outline following `references/outline-template.md`, applying confirmed theme + audience + language + slide count. Save as `outline.md`.

Stop here if `--outline-only`. Skip Step 4 if `skip_outline_review`.

### Step 4: Review Outline (Conditional)

Display a slide-by-slide table (`# | Title | Type | Layout`) along with total count and resolved theme. Ask: proceed / edit outline first / regenerate — verbatim in `references/confirmation.md`.

On "Edit outline first", tell the user to edit `outline.md` and ask again when ready. On "Regenerate outline", return to Step 3.

### Step 5: Generate Slidev Markdown

Generate a complete `slides.md` file from the outline following `references/slidev-template.md`.

The file must:
1. Open with a Slidev headmatter block (theme, title, info, language)
2. Use `---` to separate slides, with per-slide frontmatter where needed (layout, class, etc.)
3. Map each outline entry to the appropriate Slidev layout (see `references/slidev-template.md`)
4. Use Mermaid fences for diagrams, code fences with language tags for code slides
5. Add presenter notes in HTML comments (`<!-- notes -->`) for each slide
6. Use `v-click` / `v-clicks` for stepped reveals where appropriate

Backup rule applies if `slides.md` already exists.

Report progress as `Generated slide X/N`.

### Step 6: Summary

```
Slide Deck Complete!
Topic: [topic]
Theme: [theme]
Location: slide-deck/[topic-slug]/slides.md
Slides: N

To preview:
  npx slidev slide-deck/[topic-slug]/slides.md

To export PDF:
  npx slidev export slide-deck/[topic-slug]/slides.md

To export PPTX:
  npx slidev export slide-deck/[topic-slug]/slides.md --format pptx
```

## Slide Modification

| Action | How |
|--------|-----|
| Edit slide | Edit the slide content directly in `slides.md` |
| Add slide | Insert a new `---` separator and slide content at the desired position |
| Delete slide | Remove the slide's section (from `---` to the next `---`) |
| Reorder | Move slide sections within the file |

See `references/modification-guide.md` for full details.

## References

| File | Content |
|------|---------|
| `references/confirmation.md` | Verbatim AskUserQuestion option copy for every confirmation |
| `references/analysis-framework.md` | Content analysis framework |
| `references/outline-template.md` | Outline structure |
| `references/slidev-template.md` | Slidev markdown generation rules |
| `references/design-guidelines.md` | Audience and content guidelines |
| `references/content-rules.md` | Content guidelines |
| `references/modification-guide.md` | Edit/add/delete workflows |
| `references/config/preferences-schema.md` | EXTEND.md schema |

## Notes

- Generation is sequential; report progress between slides.
- Keep slide content focused: 2-3 key points per slide for balanced density.
- Use Mermaid diagrams for processes, flows, and relationships instead of text lists where possible.
- Presenter notes should explain the slide's context and key talking points.

## Extension Support

Custom configurations via EXTEND.md. See **Step 1.1** for paths and supported options.

## Changing Preferences

EXTEND.md lives at the first matching path listed in Step 1.1. Two ways to change it:

- **Edit directly** — open EXTEND.md and change fields. Full schema: `references/config/preferences-schema.md`.
- **Common one-line edits**:
  - `preferred_theme: default` — set a specific default theme.
  - `preferred_audience: experts` — pin a default audience.
  - `language: zh` — always output in Chinese.
  - `review: false` — skip outline review by default.
