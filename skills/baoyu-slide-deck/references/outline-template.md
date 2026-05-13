# Outline Template

Standard structure for slide deck outlines used to drive Slidev markdown generation.

## Outline Format

```markdown
# Slide Deck Outline

**Topic**: [topic description]
**Theme**: [Slidev theme name]
**Audience**: [target audience]
**Language**: [output language]
**Slide Count**: N slides
**Generated**: YYYY-MM-DD HH:mm

---

[Slide entries follow...]
```

## Cover Slide Template

```markdown
## Slide 1 of N

**Type**: Cover
**Layout**: cover

// NARRATIVE GOAL
[What this slide achieves in the story arc]

// KEY CONTENT
Title: [main title]
Subtitle: [supporting tagline or author/date line]

// PRESENTER NOTES
[Context and talking points for the presenter]
```

## Content Slide Template

```markdown
## Slide X of N

**Type**: Content
**Layout**: [default | two-cols | image-right | center | ...]

// NARRATIVE GOAL
[What this slide achieves in the story arc]

// KEY CONTENT
Headline: [main message — narrative, not a label]
Body:
- [point 1 with specific detail]
- [point 2 with specific detail]
- [point 3 with specific detail]

// VISUAL (optional)
[Describe any diagram, Mermaid chart, or code block to include]

// PRESENTER NOTES
[Context and talking points for the presenter]
```

## Section Divider Slide Template

```markdown
## Slide X of N

**Type**: Section
**Layout**: section

// KEY CONTENT
Title: [section name]
Description: [one-line description of what follows]
```

## Closing Slide Template

```markdown
## Slide N of N

**Type**: Closing
**Layout**: end

// NARRATIVE GOAL
[Meaningful close — call-to-action, key takeaway, or next steps]

// KEY CONTENT
Headline: [memorable closing statement or call-to-action]
Body: [optional summary points or next steps]

// PRESENTER NOTES
[Closing remarks and Q&A prompts]
```

## Slide Numbering

- Cover is always Slide 1
- Section dividers separate major topics
- Closing is always the final slide (N)

## Available Slidev Layouts

Specify these in the `**Layout**:` line of each outline entry. See [Slidev layouts docs](https://sli.dev/builtin/layouts) for details.

| Layout | Use for |
|--------|---------|
| `cover` | Title/cover slide |
| `default` | Standard content slide |
| `center` | Centered single message |
| `two-cols` | Side-by-side content (use `::right::` marker) |
| `two-cols-header` | Header + two columns |
| `image-right` | Text left, image right |
| `image-left` | Image left, text right |
| `section` | Section divider |
| `quote` | Prominent quotation |
| `fact` | Single key statistic or fact |
| `statement` | Bold single-sentence statement |
| `end` | Closing slide |

## Section Dividers

Use `---` between slide entries in the outline file.

## Slug Rules

Generate slugs for reference in notes and cross-links:

| Slide Type | Slug pattern | Example |
|------------|-------------|---------|
| Cover | `cover` | |
| Content | `{topic-keyword}` | `problem-statement` |
| Section | `section-{name}` | `section-architecture` |
| Closing | `closing` | |
