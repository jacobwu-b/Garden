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
- A plant's images live in the same directory, named after the record (see [Images](#images)).

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
| `acquired` | `YYYY-MM` or `YYYY-MM-DD` (or bare `YYYY` if the month is genuinely unknown) | When it entered your care (planted date, or acquisition date). |
| `status` | string | Short current state, e.g. `thriving`, `struggling`, `establishing`, `dormant`. |

Optional fields: `container` (`true` for portable planters/pots), `quantity` (for groupings like bulbs), `cover` (filename of the gallery thumbnail — see [Images](#images)), and any others a plant genuinely needs. Don't add a field that isn't useful; don't omit a required one.

### `origin`

- `planted` — you started or established it. **Volunteer / self-seeded** plants you chose to keep are `planted`, with a note in the body explaining the natural origin.
- `inherited` — present before you (previous tenant, or a communal/shared plant).

### Dates

Always absolute — `YYYY-MM-DD` when known, `YYYY-MM` when the day is unknown, and bare `YYYY` only when the month is genuinely unknown (common for `inherited` plants whose history predates you). Never relative ("last spring").

## Body structure

After the frontmatter, use these sections (omit any that don't apply — no empty placeholders):

```markdown
## Context
How it came to be here, identity notes, anything that explains the frontmatter.

## Photos
Images of the plant, newest first, each captioned with its date. Omit if there are none.

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

## Images

- **Format:** JPEG, ≤1600px on the long edge, with **all metadata stripped**. A plant may have several
  images over its life; that's encouraged.
- **Naming:** `{record-slug}-{YYYY-MM}.jpg` — the record's filename stem, then the capture month
  (e.g. `desert-gold-peach-2026-06.jpg`). Two in the same month get a numeric suffix: `…-2026-06-2.jpg`.
- **Location:** in the same directory as the record. Referenced from the record's `## Photos` section.
- **Gallery thumbnail:** the optional `cover` frontmatter field names which image represents the plant
  in the README gallery; if unset, the newest image is used.
- **How to add one — always via the pipeline.** Phone photos are HEIC and embed GPS coordinates that a
  plain converter does **not** remove. Run `python3 scripts/garden.py convert <src> <record.md>`, which
  decodes, strips all metadata, resizes, and writes the correctly-named JPEG. Then run
  `python3 scripts/garden.py gallery` to refresh the README grid. See `CLAUDE.md` §7.

## Privacy

This repository is public. Keep location coarse (`front` / `side` / `back`). Never record a street
address or precise coordinates.

**Images carry location too.** iPhone HEIC files embed the exact GPS coordinates where the photo was
taken. Never commit a raw HEIC (`*.heic` is git-ignored), and never commit an image with intact
metadata — always go through `scripts/garden.py convert`, which strips it. `scripts/garden.py check`
(run locally and in CI) blocks any committed HEIC or any image that still carries location data.
