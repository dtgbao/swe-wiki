# SWE Knowledge Vault

Treat this workspace as an Obsidian-compatible software engineering knowledge vault.

## Wiki operations

Use `$maintain-swe-wiki` from `.agents/skills/maintain-swe-wiki/` whenever a task involves:

- ingesting articles, documents, repositories, or notes;
- answering questions from the SWE wiki;
- filing durable syntheses or comparisons;
- checking contradictions, stale claims, citations, links, or index coverage;
- maintaining or linting files under `raw/` or `wiki/`.

Follow the skill and its references as the authoritative schema and workflow. Do not duplicate or override their detailed conventions here.

## Required boundaries

- Treat `raw/` as immutable evidence. Never edit or replace an existing source snapshot.
- Treat `wiki/` as agent-maintained and user-reviewed. Present proposed wiki changes and obtain explicit approval before writing them.
- Read `wiki/index.md` before opening topic pages for a wiki query.
- Keep claim-level provenance through source-note wikilinks and report conflicting or missing evidence explicitly.
- After an approved wiki mutation, update affected cross-links and `wiki/index.md`, then append the corresponding entry to `wiki/log.md`.
