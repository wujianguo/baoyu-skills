# Slide Modification Guide

Workflows for modifying the generated `slides.md` after initial generation.

## Edit Single Slide

Update a slide's content directly in `slides.md`:

1. Open `slides.md` and locate the slide by its heading or position
2. Edit the content between the `---` separators
3. Update `outline.md` to match if the headline changes significantly
4. Run `npx slidev slides.md` to preview

## Add New Slide

Insert a new slide at the desired position:

1. Locate the insertion point in `slides.md` (between two `---` separators)
2. Insert a new `---` separator followed by the slide content
3. Add optional per-slide frontmatter (layout, class, etc.) immediately after the `---`
4. Update `outline.md` with a new slide entry in the correct position

Example insertion:

```markdown
---
layout: default
---

# New Slide Title

- Point one
- Point two

<!--
Presenter notes for the new slide.
-->
```

## Delete Slide

Remove a slide and its separator:

1. Locate the slide section in `slides.md`
2. Delete from its opening `---` separator through to (but not including) the next `---`
3. Update `outline.md` to remove the slide entry

## Reorder Slides

1. Move the full slide block (from one `---` to the next `---`) to the new position
2. Update `outline.md` to reflect the new order

## Per-Slide Frontmatter

Add a YAML block immediately after a slide separator to configure the slide:

```markdown
---
layout: two-cols
class: text-sm
---
```

Common frontmatter fields:

| Field | Values | Purpose |
|-------|--------|---------|
| `layout` | cover, default, two-cols, center, … | Slide layout |
| `class` | any Tailwind/UnoCSS class | Extra styling |
| `transition` | slide-left, fade, … | Enter transition |
| `background` | URL or color | Slide background |

## Post-Modification Checklist

After any modification:

- [ ] `slides.md` saved and syntactically valid (no broken `---` separators)
- [ ] `outline.md` updated to reflect changes
- [ ] Preview confirmed with `npx slidev slides.md`
