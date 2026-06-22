# Wiki schema

Apply these conventions to files under `wiki/`.

## Directory taxonomy

| Page type | Directory | Purpose |
| --- | --- | --- |
| `source` | `wiki/sources/` | Traceable summary of one immutable raw snapshot |
| `concept` | `wiki/concepts/` | Durable idea independent of one implementation |
| `technology` | `wiki/technologies/` | Language, framework, library, platform, or tool |
| `pattern` | `wiki/patterns/` | Repeatable architecture, implementation, or operational approach |
| `project` | `wiki/projects/` | Knowledge specific to a software project or system |
| `synthesis` | `wiki/syntheses/` | Cross-source comparison, decision, explanation, or analysis |

Keep this taxonomy shallow. Express frontend, backend, database, infrastructure, security, testing, and tooling concerns with tags and links rather than nested folders.

## File names and links

- Use descriptive lowercase kebab-case file names, such as `server-components.md`.
- Use an Obsidian wikilink without a path when the basename is unique: `[[server-components]]`.
- Add a heading anchor for precise evidence when stable: `[[source-name#Key claims]]`.
- Rename a page only after checking and proposing all inbound links and its index entry.

## Frontmatter

Start every generated page with exactly these fields:

```yaml
---
type: concept | technology | pattern | project | synthesis | source
aliases: []
tags: []
status: active | evolving | superseded
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---
```

- Use one allowed `type` value and store the page in its matching directory.
- Use lowercase tags from the cross-cutting concerns above, adding specific tags only when useful for retrieval.
- Set `status: evolving` when evidence is incomplete or conflicting.
- Set `status: superseded` only when the page points to its replacement and explains why.
- Store source-note wikilinks in `sources`; use `[]` only when the page contains no external factual claims.
- Change `updated` only when page meaning changes, not for formatting-only edits.

## Page bodies

Use a single H1 title followed by the minimum useful sections. Put citations directly after substantive claims. Clearly label inference or synthesis that is not directly stated by a source.

A source page must contain:

```markdown
# Source title

## Origin
- Raw snapshot: `raw/<category>/<file>`
- Original: <URL, repository, document, or supplied origin>
- Captured: YYYY-MM-DD

## Summary

## Key claims

## Related pages
```

Never cite a raw path directly from a knowledge page. Cite its `wiki/sources/` note so readers can inspect normalized provenance and then follow the raw-snapshot path.

## Claim and conflict handling

- Place `[[source-note]]` or `[[source-note#Heading]]` after each sourced claim.
- When sources disagree, retain both positions, cite each one, describe the disagreement, and set the affected page to `evolving`.
- When newer evidence replaces an old conclusion, preserve the old source and explain the transition. Do not rewrite history.

## Index contract

Keep `wiki/index.md` grouped under Sources, Concepts, Technologies, Patterns, Projects, and Syntheses. Represent every page exactly once:

```markdown
- [[page-name]] — One-line description. `tags: frontend, testing` · updated YYYY-MM-DD · 2 sources
```

Use `0 sources` only for project-local or explicitly unsourced pages. Sort entries alphabetically within each section. Remove an index entry only when its page is deliberately removed or replaced.

## Log contract

Treat `wiki/log.md` as append-only. Add entries in chronological order using local time:

```markdown
## [YYYY-MM-DD HH:mm +07:00] ingest | Title

- Inputs: paths or URLs
- Proposed: created and updated pages
- Applied: created and updated pages, or `none`
- Contradictions: summary or `none`
- Gaps: summary or `none`
- Approval: approved, partial, rejected, or not requested
```

Use operation names `ingest`, `query`, or `lint`. Append one entry per completed operation, including reviewed operations that result in no file changes. Never edit earlier entries.
