# SWE Knowledge Vault

This is my personal, agent-maintained knowledge base for software engineering. It turns articles, documents, repositories, and notes into persistent, linked knowledge that becomes more useful over time.

## What this is

Most document tools retrieve source fragments and reconstruct an answer for every question. This vault instead compiles useful information into a maintained wiki. New sources can strengthen existing pages, introduce concepts, expose contradictions, or revise an earlier synthesis.

The model comes from Karpathy's [LLM Wiki pattern](https://gist.githubusercontent.com/karpathy/442a6bf555914893e9891c11519de94f/raw/ac46de1ad27f92b28ac95459c782c07f6b8c964a/llm-wiki.md): I curate the material and direct the investigation; the agent handles filing, linking, provenance, and maintenance.

## Quick start

1. Choose one useful source—a URL, document, repository, or note—and give it to Codex.
2. Ask Codex to use `$maintain-swe-wiki` to ingest it. The agent will inspect the existing index and present the takeaways, conflicts, and proposed file changes.
3. Review the proposal and give explicit approval for the changes I want. Only then should the agent update the wiki, index, cross-links, and activity log.

Example:

```text
Use $maintain-swe-wiki to ingest this article into my SWE wiki: <URL>.
Show me the key claims, contradictions, and affected pages before writing anything.
```

## How the vault is organized

| Path | Responsibility |
| --- | --- |
| `raw/` | Immutable snapshots of original evidence, grouped by source kind |
| `wiki/` | Agent-maintained knowledge pages that synthesize the sources |
| `wiki/index.md` | Content catalog and first stop for every wiki query |
| `wiki/log.md` | Append-only history of ingests, queries, and lint passes |
| `AGENTS.md` | Vault-level rules that route wiki work through the skill |
| `.agents/skills/maintain-swe-wiki/` | Authoritative operating workflow, schema, and skill metadata |

Wiki pages are grouped by purpose—sources, concepts, technologies, patterns, projects, and syntheses. Cross-cutting concerns such as frontend, backend, databases, infrastructure, security, testing, and tooling are connected through tags and wikilinks rather than deep folder trees.

## Core workflows

### Ingest

Add one source deliberately. The agent preserves a raw snapshot, checks related knowledge, and proposes the pages and links that should change. I approve the write set before the wiki is updated.

### Query

Ask a question against the compiled wiki. The agent reads `wiki/index.md` first, follows relevant pages and citations, and reports conflicts or evidence gaps instead of inventing certainty.

### Synthesize

Turn a useful answer, comparison, or architectural conclusion into a durable synthesis page. Filing is separate from answering: the agent proposes the page and waits for approval.

### Lint

Periodically inspect the vault for missing index entries, broken links, orphan pages, stale claims, hidden contradictions, weak citations, and knowledge gaps. Repairs also require review before writing.

## Useful prompts

Ingest a source:

```text
Use $maintain-swe-wiki to ingest <source>. Propose the raw snapshot, source note,
affected pages, contradictions, and index changes before writing.
```

Ask a cross-cutting question:

```text
Use $maintain-swe-wiki to explain how authentication flows through the frontend,
API, and database in my current knowledge base. Cite the supporting source notes
and call out anything the wiki cannot establish.
```

Preserve a useful result:

```text
Propose filing this answer as a synthesis page. Show the outline, citations,
related wikilinks, and index entry before creating it.
```

Check wiki health:

```text
Use $maintain-swe-wiki to lint the entire wiki. Prioritize contradictions,
uncited claims, broken links, orphan pages, and missing index entries. Propose
repairs without applying them.
```

## Make the most of it

- **Ingest deliberately.** One strong source with careful integration is usually more valuable than a large unsupervised batch.
- **Ask questions across boundaries.** Connections between frontend, backend, data, infrastructure, and operations are where a maintained wiki compounds in value.
- **Review proposals critically.** Correct emphasis, page placement, and terminology before approving writes.
- **Keep disagreement visible.** Conflicting sources are useful information; preserve both positions and the evidence behind them.
- **File durable discoveries.** Save comparisons, decisions, and explanations that will matter again instead of leaving them in chat history.
- **Lint periodically.** Run a health check after several ingests or whenever navigation and retrieval start feeling unreliable.
- **Improve the workflow as patterns emerge.** Update the skill conventions when repeated friction reveals a better way to organize or maintain the vault.

## Operating rules

- Raw snapshots are immutable. A changed source becomes a new snapshot rather than replacing history.
- Wiki changes require explicit approval after the proposed write set is shown.
- Queries begin with `wiki/index.md` and expand only into relevant pages.
- Substantive claims keep claim-level provenance through source-note wikilinks.
- Contradictory, incomplete, and superseded claims remain visible and clearly marked.
- Completed operations update the index where needed and append one entry to `wiki/log.md`.

## Further reading

- [LLM Wiki pattern](https://gist.githubusercontent.com/karpathy/442a6bf555914893e9891c11519de94f/raw/ac46de1ad27f92b28ac95459c782c07f6b8c964a/llm-wiki.md)
- [Vault instructions](AGENTS.md)
- [Maintain SWE Wiki skill](.agents/skills/maintain-swe-wiki/SKILL.md)
- [Wiki schema](.agents/skills/maintain-swe-wiki/references/wiki-schema.md)
- [Wiki workflows](.agents/skills/maintain-swe-wiki/references/workflows.md)
