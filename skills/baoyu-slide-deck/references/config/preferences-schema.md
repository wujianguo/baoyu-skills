# EXTEND.md Schema

Structure for user preferences in `.baoyu-skills/baoyu-slide-deck/EXTEND.md`.

## Full Schema

```yaml
# Slide Deck Preferences

## Defaults
preferred_theme: default      # Any Slidev theme name (default, seriph, apple-basic, …)
audience: general             # beginners | intermediate | experts | executives | general
language: auto                # auto | en | zh | ja | etc.
review: true                  # true = review outline before generation
```

## Field Descriptions

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `preferred_theme` | string | `default` | Slidev theme to use when no theme is specified in the request. Set to a valid Slidev theme name. |
| `audience` | string | `general` | Default target audience |
| `language` | string | `auto` | Output language (auto = detect from input) |
| `review` | boolean | `true` | Show outline review before generating slides |

## Minimal Examples

### Just change default theme

```yaml
preferred_theme: seriph
```

### Prefer no review

```yaml
review: false
```

### Experts audience, Chinese output

```yaml
audience: experts
language: zh
```

## File Locations

Priority order (first found wins):

1. `.baoyu-skills/baoyu-slide-deck/EXTEND.md` (project)
2. `$XDG_CONFIG_HOME/baoyu-skills/baoyu-slide-deck/EXTEND.md` (XDG config)
3. `$HOME/.baoyu-skills/baoyu-slide-deck/EXTEND.md` (user)
