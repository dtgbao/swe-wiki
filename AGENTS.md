# SWE Wiki Agent Instructions

Use the `$swe-wiki` skill for setup, ingestion, durable queries, linting, and
sync. This repository is the persistent knowledge base; treat its Markdown as
maintained data, not disposable notes.

## Invariants

- `raw/` is immutable source material.
- `wiki/` is the synthesis layer. Cite source URLs, raw paths, or source pages.
- Update an existing durable page before creating a near-duplicate.
- Keep `wiki/index.md` complete, concrete, and organized by page kind.
- Keep `wiki/log.md` append-only. Valid events are `bootstrap`, `ingest`,
  `query`, and `lint`; Git history records syncs.
- Use stable relative Markdown links.
- Add Mermaid diagrams to decisions and blueprints when relationships, flows,
  boundaries, or lifecycle matter.
- Keep credentials and machine-local configuration out of the repository.

Every page under `wiki/` except `index.md` and `log.md` requires:

```yaml
---
title: "Readable title"
kind: concept
status: draft
tags: [swe]
sources: []
updated: YYYY-MM-DD
confidence: medium
---
```

Allowed `kind` values: `source`, `concept`, `decision`, `blueprint`, `practice`,
`convention`, `system`, `question`.

Allowed `status` values: `draft`, `evergreen`, `superseded`. Allowed
`confidence` values: `high`, `medium`, `low`.

## Completion gates

### Ingest

- The entire source, including code, diagrams, tables, footnotes, and available
  local images, was read.
- Its source page has provenance, summary, extracted SWE atoms, impacted pages,
  and open questions.
- Reusable knowledge was merged into the correct durable pages.
- Every changed page appears in `wiki/index.md`.
- Contradictions or superseded claims are recorded.
- A parseable `ingest` entry was appended to `wiki/log.md`.

### Query

- Start from `wiki/index.md`, then search and read relevant wiki pages.
- Prefer synthesized pages; use raw sources when the wiki points to them or is
  insufficient.
- Cite wiki paths in the answer.
- Save independently useful synthesis under `wiki/questions/`, update the
  index, and append a `query` log entry.

### Lint

- Mechanical lint passes or every remaining error is reported.
- Semantic review covers contradictions, staleness, orphans, missing pages,
  provenance, cross-links, index summaries, and architecture diagrams.
- Fix in-scope drift and append a parseable `lint` log entry.

### Sync

- Run only after an explicit user request.
- Review the exact pending wiki changes first.
- Commit, fetch, rebase, and push without force.
- If rebase conflicts occur, abort the rebase and ask the user to resolve them.
