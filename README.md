# SWE Wiki

A persistent, Git-synced software engineering knowledge base. Sources preserve
provenance; the wiki turns them into reusable concepts, decisions, blueprints,
practices, conventions, and system notes.

Start with [the content index](wiki/index.md). Use
[the activity log](wiki/log.md) to see when knowledge was ingested, queried, or
linted.

## Knowledge model

- `raw/` holds immutable local source material.
- `wiki/sources/` records one source's provenance, summary, extracted knowledge,
  impacted pages, and open questions.
- Durable pages synthesize knowledge that should remain useful independently:

| Directory | Content |
| --- | --- |
| `concepts/` | Ideas, tradeoffs, algorithms, patterns, and failure modes |
| `decisions/` | ADR-like decisions, alternatives, and consequences |
| `blueprints/` | Reusable architectures, protocols, data flows, and build plans |
| `practices/` | Checklists, testing strategies, and operational playbooks |
| `conventions/` | Coding, API, logging, documentation, and naming rules |
| `systems/` | Frameworks, libraries, services, tools, and vendors |
| `questions/` | Durable answers produced while querying the wiki |

Every durable claim should lead back to its source. Prefer updating an existing
page over creating a near-duplicate.

## Repository layout

```text
.
├── README.md
├── AGENTS.md
├── raw/
├── wiki/
│   ├── index.md
│   ├── log.md
│   ├── sources/
│   ├── concepts/
│   ├── decisions/
│   ├── blueprints/
│   ├── practices/
│   ├── conventions/
│   ├── systems/
│   └── questions/
└── .obsidian/          # Obsidian vault settings
```

The repository root can be opened directly as an Obsidian vault, but all
knowledge remains plain Markdown and works without Obsidian.

## Working with the wiki

Use the `$swe-wiki` skill for maintained operations:

- **Ingest:** read one source fully, preserve its provenance, and update reusable
  pages.
- **Query:** search the index and existing synthesis before consulting raw
  sources.
- **Lint:** check schema, links, index coverage, contradictions, staleness,
  provenance, and diagrams.
- **Sync:** commit wiki changes, rebase on the configured remote branch, and push
  without force. Sync only when explicitly requested.

Repository-specific rules and completion checks live in [AGENTS.md](AGENTS.md).
