# Garden

A personal tracker for my garden in San Jose, CA. Each plant has a Markdown record covering what it
is, where it lives, where it came from, how it's doing, and how I care for it.

## How it's organized

```
plants/
  front/   side/   back/    # one file per plant, by location
  _TEMPLATE.md               # start a new record from this
docs/
  conventions.md             # record schema and naming rules
CLAUDE.md                    # working conventions for the repo
```

Records are Markdown with YAML frontmatter; images live beside the record they document.

## Adding or updating a plant

1. Read [`docs/conventions.md`](docs/conventions.md).
2. Copy [`plants/_TEMPLATE.md`](plants/_TEMPLATE.md) into the right location directory.
3. Fill the frontmatter and the sections that apply.
4. Open a PR — see [`CLAUDE.md`](CLAUDE.md) for the branch/PR flow.

## A note on privacy

This repository is public. Locations stay coarse (front / side / back), and records never contain a
street address, precise coordinates, or images with embedded location data.
