---
name: Common name
species: Botanical or cultivar name
location: front # front | side | back — must match the directory
origin: planted # planted | inherited
acquired: YYYY-MM # or YYYY-MM-DD; bare YYYY if month genuinely unknown
status: establishing # short current state
# optional: container: true | quantity: N | cover: <slug>-YYYY-MM.jpg
---

## Context

How it came to be here, identity notes, anything that explains the frontmatter.

## Photos

<!-- Omit this section if there are no photos. Add images via:
     python3 scripts/garden.py convert <src.HEIC> <this-record.md>
     then: python3 scripts/garden.py gallery -->

![Common name — YYYY-MM](slug-YYYY-MM.jpg)
*YYYY-MM*

## Needs

Sun, water, soil, climate notes — what this plant wants in this spot.

## Maintenance

Recurring care: pruning, compost, watering cadence, fungicide, etc.

## Log

Reverse-chronological dated observations; required whenever state changes.

- YYYY-MM: most recent event.
- YYYY-MM: planted / acquired.
