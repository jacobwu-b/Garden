# Garden — Claude Code Instructions

The conventions are the source of truth. Records are the contract. This file is the operating system.
Read it before every session. Rules are not suggestions.

This is a **content repository**, not a software project: plant records in Markdown, plus images. There is little or no code — at most lightweight validation of file names and record invariants. Where the original engineering template spoke of "tests," "builds," and "schemas," this file substitutes **record conventions** and **validation of those conventions**. The discipline is the same; the artifacts differ.

---

## Non-negotiables

1. Branch from main, squash-merge to main. Branches never touch other branches.
2. **This repo is public.** No precise home address, no fine-grained geolocation, no personal/sensitive data — in records, commit messages, or image metadata. Locations stay coarse: `front` / `side` / `back`.
3. Conventions → Plan → Records. Establish or confirm the convention before adding content above Trivial.
4. A record is done when it conforms to the convention and validation (if any) is green → auto-open PR.
5. No AI attribution anywhere in git history.
6. When in doubt, stop and surface — don't work around.

---

## 1. Project Context

- **What:** A personal tracker for the garden — what's planted, where, its origin and history, how it's doing, and its care schedule.
- **Conventions:** `docs/conventions.md` — the record schema and naming rules. Read it before adding or restructuring records.
- **Format:** Markdown records with YAML frontmatter, organized by location. Images live beside the records they document.
- **Validation:** none yet (manual review). If a check script is added later, it lives in `scripts/` and is documented here.

---

## 2. Working Philosophy

These four govern every decision below.

**Think before writing.** State assumptions. If a plant's origin, date, or identity is ambiguous, name the ambiguity in the record (e.g. `species: "Fig — cultivar unknown"`) rather than inventing precision.

**Simplicity first.** Record what's true and useful for tending the garden. No speculative fields, no empty sections kept "for symmetry." If a section doesn't apply to a plant, omit it.

**Surgical changes.** Touch only the records the task requires. Don't reformat or "tidy" unrelated records, don't restructure the tree on a whim. Every changed line should trace to the request.

**Goal-driven execution.** Convert each task into a verifiable goal. "Log the peach harvest" → "the Desert Gold Peach record has a dated log entry for the harvest." "Add the new plant" → "a record exists at the conventional path with all required frontmatter."

---

## 3. Session Start

Run `git checkout main && git pull origin main`, then `git status` and `git branch`. If anything is unexpected (uncommitted changes, untracked files you didn't create), stop and report.

Then, before any work above Trivial:
1. Read this file end-to-end.
2. Read `docs/conventions.md` and `plants/_TEMPLATE.md`.
3. If a location or plant has its own notes that constrain the change, read them too.

**Definition of Ready:** state in one sentence what record(s) change and what the change is. If either is unclear, ask.

**Triage** — when in doubt, treat as one tier larger.

| Tier | Criteria | Process |
|---|---|---|
| **Trivial** | One record, reversible: a log entry, a status/date correction, a typo, adding one image | Proceed directly. Auto-PR on green. |
| **Standard** | A handful of records, or one new plant record following the existing convention | Brief plan → Convention/record loop → auto-PR. |
| **Significant** | Many records at once, a change to the convention or directory layout, a new field added across records, or anything not easily reversible | Discuss in chat *first*. Note the rationale (an ADR-style note in `docs/decisions/` if non-obvious). Then plan → approval. |

---

## 4. The Loop: Conventions → Plan → Records

**Trivial work skips this. Standard and Significant always run it.**

1. **Conventions.** Confirm the change against `docs/conventions.md` and `plants/_TEMPLATE.md`. If the convention is silent or contradicts the request, stop and surface — don't infer a new field or path silently. A genuinely new field or layout is a convention change (Significant): update `docs/conventions.md` first.
2. **Plan.** Produce the format below. For Significant work, save it under `docs/plans/` and wait for approval.
3. **Records.** Make the minimum change that satisfies the goal. Frontmatter complete, dates absolute (`YYYY-MM` or `YYYY-MM-DD`), location coarse.
4. **Tidy.** Re-read the diff. Remove anything you added that the task didn't need.
5. **Validate.** Filenames follow convention, required frontmatter present, no sensitive data. Green → auto-open PR (§5).

### Plan format

```
Branch:  {type}/{scope}-{description}
What:    [1 sentence]
Records: [paths — create/modify]
Notes:   [convention impact, or "conforms to existing convention"]
```

For Significant work, add **Blast radius** (which records / which fields) and **Open questions**.

If something surfaces mid-task that wasn't planned (an ambiguous identity, a convention gap): stop and report. No unilateral convention changes.

---

## 5. Git & PR Protocol

**Invariant:** every branch is born from the tip of main and dies by squash-merge into main. A merge conflict means this rule was broken — stop and report, do not resolve.

**Branch:** `{type}/{scope}-{description}`, kebab-case. Types: `feat` (new plant/area) `fix` (correction) `chore` `docs` `refactor` (restructuring records).

**PR title** = squash commit on main. Conventional Commits: `{type}({scope}): {imperative, ≤72 chars}`.

**Auto-PR on green.** When the change conforms to the convention and validation passes, push and open the PR immediately using `.github/PULL_REQUEST_TEMPLATE.md` — fill every section, no placeholders. Post the URL. Do not merge unless asked; merging is the owner's call.

**Attribution:** zero AI attribution, co-author tags, or agent signatures. Anywhere. No "🤖 Generated with…" footers.

**Aborting a branch:** close PR, `git branch -D {branch}`. Unmerged work is discarded.

---

## 6. Record Conventions (invariants)

Changing any of these is a Significant change requiring approval *before* records are written. The authoritative detail lives in `docs/conventions.md`; the invariants are:

- **Layout:** one Markdown file per plant under `plants/{location}/`, where location ∈ `front`, `side`, `back`. Images for a plant live alongside it.
- **Filenames:** kebab-case, descriptive, disambiguated when a species repeats (e.g. `fig-air-layer.md` vs `fig-communal.md`). Lowercase, no spaces.
- **Frontmatter:** every record carries the required fields defined in `plants/_TEMPLATE.md` (at minimum `name`, `species`, `location`, `origin`, a planted/acquired date, `status`).
- **`origin`:** exactly one of `planted` (I started or established it) or `inherited` (present before me — previous tenant or communal). Volunteer/self-seeded plants I chose to keep are `planted` with a note.
- **Dates:** absolute, `YYYY-MM` or `YYYY-MM-DD`. No "last spring," no relative dates.
- **Privacy:** coarse location only; never a street address or precise coordinates. Strip location metadata from images before committing.

---

## 7. Validation

There is no automated test suite. A record is valid when:

| Change | Required check |
|---|---|
| New plant record | Conventional path; all required frontmatter present; renders cleanly |
| Edit to a record | Frontmatter still valid; new dated log entry if state changed |
| New image | Lives beside its record; referenced from the record; no location metadata |
| Convention change | `docs/conventions.md` and `plants/_TEMPLATE.md` updated in the same PR |

If a validation script is later added to `scripts/`, document its command here and run it before every PR.

**Prohibitions:** don't invent dates or identities to fill a field — mark unknowns explicitly. Don't commit images with embedded location data. Don't loosen the convention to fit a record; fix the record or change the convention deliberately.

---

## 8. Issues

Use GitHub Issues for work that **isn't the current task**: a plant that needs attention later, a pest to watch, a convention gap you noticed but shouldn't fix mid-task, a record you suspect is wrong but can't confirm. Don't silently fix out-of-scope things; don't expand the current PR. Link from the PR's "Out of scope" section.

Don't file for the current task, or for trivial corrections you're already authorized to make.

---

## 9. Hard Prohibitions

Stop and surface before any of these:

- Commit to main (except the one-time empty root commit that bootstraps the repo); manual `merge`/`rebase`; force-push; branch from anything but main; AI attribution in git
- Commit a street address, precise coordinates, or images with location metadata
- Add a new frontmatter field or change the directory layout without updating `docs/conventions.md` first (convention change = approval)
- Invent a date, species, or origin to fill a field instead of marking it unknown
- Restructure or reformat unrelated records; fix unrelated suspected errors without asking
- Mark work done before merge is confirmed

---

## 10. Landmines

Document specific things the agent gets wrong here as they happen. Each entry: one-line description + correct behavior. Remove entries that no longer fire.

- *(none yet)*

---

## 11. Definition of Done

All of:
- The record(s) conform to `docs/conventions.md` and `plants/_TEMPLATE.md`
- Validation green (filenames, frontmatter, no sensitive data)
- PR auto-opened against main, template filled, URL posted
- Merged and confirmed

Record written ≠ done. PR opened ≠ done. **Merged and confirmed = done.**

---

## 12. Repository Map

| Path | What it's for |
|---|---|
| `CLAUDE.md` | This file. Operating system for the agent. |
| `README.md` | Human-facing overview of the garden and how records are organized. |
| `docs/conventions.md` | Source of truth for record structure and naming. Read before adding content. |
| `plants/_TEMPLATE.md` | Copy this to start a new plant record. |
| `plants/{front,side,back}/` | Plant records, one file per plant, organized by location. Images live alongside. |
| `.github/PULL_REQUEST_TEMPLATE.md` | Auto-loaded into every PR. Fill every section. |

Optional, add when first needed (and document here): `docs/decisions/` for convention ADRs, `docs/plans/` for Significant-tier plans, `scripts/` for a validation script.
