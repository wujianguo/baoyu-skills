# Confirmation Questions

Concrete option copy for the confirmation steps. SKILL.md lists which questions to ask — this file gives the verbatim options used in Claude Code. Adapt copy to the runtime's native user-input tool; the intent matters more than the exact wording.

## Round 1 (Always)

Batch all four questions in a single `AskUserQuestion` call.

### Q1: Theme

```yaml
header: Theme
question: Which Slidev theme for this deck?
options:
  - label: "{recommended_theme} (Recommended)"
    description: Best match based on content analysis
  - label: "{alternative_theme}"
    description: "{alternative theme description}"
  - label: default
    description: Clean and minimal, general purpose
```

### Q2: Audience

```yaml
header: Audience
question: Who is the primary audience?
options:
  - label: General readers (Recommended)
    description: Broad appeal, accessible content
  - label: Beginners/learners
    description: Educational focus, clear explanations
  - label: Experts/professionals
    description: Technical depth, domain knowledge
  - label: Executives
    description: High-level insights, minimal detail
```

### Q3: Slide Count

```yaml
header: Slides
question: How many slides?
options:
  - label: "{N} slides (Recommended)"
    description: Based on content length
  - label: "Fewer ({N-3} slides)"
    description: More condensed, less detail
  - label: "More ({N+3} slides)"
    description: More detailed breakdown
```

### Q4: Review Outline

```yaml
header: Outline
question: Review outline before generating slides?
options:
  - label: Yes, review outline (Recommended)
    description: Review slide titles and structure
  - label: No, skip outline review
    description: Proceed directly to slide generation
```

## Outline Review (Step 4)

```yaml
header: Confirm
question: Ready to generate slides.md?
options:
  - label: Yes, proceed (Recommended)
    description: Generate Slidev markdown
  - label: Edit outline first
    description: I'll modify outline.md before continuing
  - label: Regenerate outline
    description: Create new outline with different approach
```

## Existing Content (Step 1.3)

```yaml
header: Existing
question: Existing content found. How to proceed?
options:
  - label: Regenerate outline
    description: Keep slides.md, regenerate outline only
  - label: Regenerate slides
    description: Keep outline, regenerate slides.md only
  - label: Backup and regenerate
    description: Backup to {slug}-backup-{timestamp}, then regenerate all
  - label: Exit
    description: Cancel, keep existing content unchanged
```
