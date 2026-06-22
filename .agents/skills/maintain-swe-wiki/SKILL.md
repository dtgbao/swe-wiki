---
name: maintain-swe-wiki
description: Maintain and query a persistent Obsidian-compatible full-stack software engineering wiki backed by immutable raw sources. Use when ingesting articles, documents, repositories, or notes; answering full-stack questions from the wiki; filing durable syntheses; checking conflicting or superseded claims; or linting and repairing wiki structure, citations, links, index entries, and logs.
---

# Maintain SWE Wiki

Maintain the vault as compiled knowledge: preserve evidence in `raw/`, integrate it into `wiki/`, and use `wiki/index.md` as the entry point for retrieval.

## Establish the vault root

Treat the nearest directory containing both `raw/` and `wiki/` as the vault root. If either directory is absent, report the missing structure and propose creating it before continuing.

Use `Asia/Ho_Chi_Minh` (`+07:00`) for timestamps. Use Obsidian wikilinks for internal links.

## Load the relevant reference

- Read [references/wiki-schema.md](references/wiki-schema.md) before creating, renaming, or validating pages, index entries, citations, or log entries.
- Read [references/workflows.md](references/workflows.md) before ingesting, querying, filing a synthesis, or linting.
- Read both references when an operation can modify the wiki.

## Enforce ownership and review boundaries

- Treat `raw/` as immutable evidence. Never edit or replace an existing snapshot. Create a new dated snapshot for changed material.
- Treat `wiki/` as agent-maintained and user-reviewed. The user may read it; modify it only through this skill's workflows.
- Separate proposal from mutation. Present takeaways, contradictions, affected paths, and intended index/log changes before writing.
- Interpret approval narrowly. Apply only the proposed changes the user approved; re-propose material additions discovered during implementation.
- Do not modify the wiki while answering a question unless the user separately approves filing the result.
- Do not introduce CLI search tools, embeddings, vector databases, or external indexing.

## Retrieve progressively

1. Read `wiki/index.md` first.
2. Select pages whose summaries, tags, or page types match the request.
3. Follow only relevant wikilinks and claim-level source citations.
4. Consult `wiki/sources/` or `raw/` only when a claim is disputed, missing, or requires primary evidence.
5. State evidence gaps and conflicts. Never fill them with unsupported claims.

Keep detailed source content out of `SKILL.md`; load only the pages required for the current task.

## Preserve provenance

Place a source-note wikilink immediately after each substantive sourced claim. Prefer section anchors when the source note provides stable headings. Distinguish source claims from agent synthesis. Preserve competing claims explicitly and mark pages `evolving` or `superseded` instead of silently overwriting history.

## Complete every operation

After an approved mutation, update affected cross-links and `wiki/index.md`, then append exactly one corresponding entry to `wiki/log.md`. Never rewrite or reorder older log entries. Report created and updated paths when finished.
