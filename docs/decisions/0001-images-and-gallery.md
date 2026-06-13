# 0001 — Plant images and a README gallery

- **Status:** accepted
- **Date:** 2026-06-13

## Context

Records should carry photos, and the README should show every plant at a glance with thumbnails.
Source photos are iPhone HEIC files, which GitHub does not render in Markdown and which must be
converted to JPEG. The repository is public.

## The finding that drove the design

iPhone HEIC files **embed the exact GPS coordinates** where the photo was taken (verified:
`37.335, -121.902` on a sample). Critically, macOS `sips -s format jpeg` — the obvious converter —
**does not strip them**; the coordinates survive into the JPEG. So any naive pipeline leaks the
home's location, violating the repo's privacy non-negotiable. Committing the raw HEIC (e.g. "let a
GitHub Action convert it") is worse: the GPS-bearing original lives in git history forever.

## Decision

1. **Convert and strip locally, before commit.** A committed script, `scripts/garden.py`, decodes
   HEIC with `sips` and re-encodes with Pillow as a **brand-new image object** (zero metadata),
   auto-rotated and resized to ≤1600px. Verified output: 0 EXIF tags, no GPS.
2. **Raw HEIC is git-ignored** (`*.heic`/`*.HEIC`) and never committed.
3. **Image convention:** JPEG named `{record-slug}-{YYYY-MM}.jpg` (numeric suffix for same-month
   repeats), beside the record, embedded in a `## Photos` section. Multiple photos over a plant's
   life are expected.
4. **Gallery is generated**, not hand-maintained: `garden.py gallery` rewrites an HTML thumbnail
   grid in `README.md` between `<!-- GALLERY:START/END -->` markers, grouped by location and linking
   to each record. An optional `cover:` frontmatter field selects the thumbnail (newest photo if
   unset).
5. **CI + local guardrail:** `garden.py check` (run locally and by
   `.github/workflows/validate-images.yml`) fails if any HEIC is committed, any image carries GPS
   metadata, or the gallery is stale.

## Alternatives considered

- **Commit HEIC, convert in a GitHub Action.** Rejected: to avoid committing GPS-laden originals the
  strip must happen *before* commit anyway, so CI conversion adds moving parts without removing the
  local step — and risks the original entering history.
- **One-time manual conversion, no committed tool.** Rejected: more photos are coming; every future
  upload would repeat the manual GPS strip with no guardrail. A reusable script + CI is safer.
- **Hand-maintained gallery.** Rejected: drifts out of date as photos/plants are added.

## Consequences

- Adding a photo is always: `garden.py convert` → set `cover:`/embed → `garden.py gallery`.
- Tooling depends on Python 3 + Pillow; `convert` depends on macOS `sips` to decode HEIC.
- The privacy invariant is now machine-enforced, not just a manual review item.
