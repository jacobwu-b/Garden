# Plan — images and README gallery

```
Branch:  feat/images-and-gallery
What:    Add per-plant photos, a generated README gallery, and a HEIC→JPEG pipeline that strips
         location metadata, enforced locally and in CI.
Records: docs/conventions.md, plants/_TEMPLATE.md (convention change, same PR)
         scripts/garden.py (new: convert / gallery / check)
         .github/workflows/validate-images.yml (new: CI guardrail)
         .gitignore (ignore *.heic), README.md (gallery + "Adding photos")
         CLAUDE.md §1/§7/§12 (pipeline + validation command)
         docs/decisions/0001-images-and-gallery.md (ADR)
         7 records gain `cover:` + `## Photos`: desert-gold-peach, echeveria-setosa,
         zonal-geranium, sedum, fig-air-layer, fig-natural-propagation, amaryllis
         + the 7 converted JPEGs beside them
Notes:   Convention change (new optional `cover` field, `## Photos` section, image naming, scripts/).
         See ADR 0001 for the rationale (HEIC GPS leak; sips does not strip).
```

## Blast radius

- **Records:** 7 of 11 gain a `cover:` field and a `## Photos` section; the other 4
  (kale, moorpark-apricot, echeveria-shaviana, fig-communal) are untouched and appear in the gallery
  without a thumbnail.
- **Fields:** one new *optional* frontmatter field, `cover`.
- **Docs/tooling:** new `scripts/`, `docs/decisions/`, `docs/plans/`, `.github/workflows/`.

## Open questions

- Gallery thumbnails link to the record `.md` (chosen) rather than the full image.
