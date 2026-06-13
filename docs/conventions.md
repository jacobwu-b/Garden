# Record Conventions

The source of truth for how plant records are structured and named. Read this before adding or
restructuring records. Changing anything here is a convention change — see `CLAUDE.md` §6.

## Layout

```
plants/
  front/   # plants in the front of the property
  side/    # plants on the side
  back/    # plants in the back
```

- One Markdown file per plant.
- Location is the directory, not a guess — `front`, `side`, `back` only.
- A plant's images live in the same directory, named after the record (e.g. `desert-gold-peach-2026-06.jpg`).

## Filenames

- kebab-case, lowercase, no spaces: `desert-gold-peach.md`.
- Descriptive enough to identify the plant at a glance.
- **Disambiguate when a species repeats.** There are multiple figs; the filename says which one:
  `fig-natural-propagation.md`, `fig-air-layer.md`, `fig-communal.md`.

## Frontmatter

Every record begins with YAML frontmatter. Required fields:

| Field | Values | Notes |
|---|---|---|
| `name` | string | Common name as you refer to it. |
| `species` | string | Botanical or cultivar name. Mark unknowns: `"Fig — cultivar unknown"`. |
| `location` | `front` \| `side` \| `back` | Must match the directory. |
| `origin` | `planted` \| `inherited` | See below. |
| `acquired` | `YYYY-MM` or `YYYY-MM-DD` | When it entered your care (planted date, or acquisition date). |
| `status` | string | Short current state, e.g. `thriving`, `struggling`, `establishing`, `dormant`. |

Optional fields: `container` (`true` for portable planters/pots), `quantity` (for groupings like bulbs), and any others a plant genuinely needs. Don't add a field that isn't useful; don't omit a required one.

### `origin`

- `planted` — you started or established it. **Volunteer / self-seeded** plants you chose to keep are `planted`, with a note in the body explaining the natural origin.
- `inherited` — present before you (previous tenant, or a communal/shared plant).

### Dates

Always absolute — `YYYY-MM` when the day is unknown, `YYYY-MM-DD` when known. Never relative ("last spring").

## Body structure

After the frontmatter, use these sections (omit any that don't apply — no empty placeholders):

```markdown
## Context
How it came to be here, identity notes, anything that explains the frontmatter.

## Needs
Sun, water, soil, climate notes — what this plant wants in this spot.

## Maintenance
Recurring care: pruning, compost, watering cadence, fungicide, etc.

## Log
Reverse-chronological dated observations. One bullet per event.
- 2026-06: first harvest — small but delicious.
- 2025-03: planted as a sapling.
```

A **`## Log`** entry is required whenever a record's state changes — it's the history that makes the tracker useful.

## Privacy

This repository is public. Keep location coarse (`front` / `side` / `back`). Never record a street
address or precise coordinates. Strip location metadata from images before committing.
