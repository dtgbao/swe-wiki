# Wiki workflows

Use the matching workflow and maintain a hard review boundary between proposal and mutation.

## Ingest

### Discovery and proposal

1. Identify the input, its origin, and the correct `raw/` category.
2. Check whether an identical snapshot or source note already exists. Do not overwrite either.
3. Copy external content into a dated immutable snapshot. For already-local content, propose the destination before copying it into `raw/`.
4. Read `wiki/index.md`, then only likely related wiki pages.
5. Extract key claims, scope, limitations, and full-stack relevance.
6. Identify new pages, existing pages to revise, links to add, contradictions, and unresolved gaps.
7. Present the proposed raw path, source note, affected wiki paths, index changes, and log title. Stop before any wiki mutation and request approval.

Creating the raw snapshot may occur only when the user explicitly asked to ingest the source. It does not imply approval to modify `wiki/`.

### Approved application

1. Apply only approved pages and claims using the schema.
2. Add claim-level citations and reciprocal links where each direction helps navigation.
3. Preserve conflicting claims and update status appropriately.
4. Update each changed page's `updated` date.
5. Add or revise every affected `wiki/index.md` entry.
6. Append one `ingest` entry to `wiki/log.md`, including partial or rejected parts.
7. Report created and updated paths plus remaining gaps.

## Query

1. Read `wiki/index.md` before any topic page.
2. Select relevant entries by description, tags, and type.
3. Read the smallest sufficient set of pages and follow only relevant links.
4. Consult source notes or raw snapshots when evidence is disputed, absent, or needs verification.
5. Answer using claim-level source-note wikilinks. Separate sourced facts, synthesis, conflicts, and gaps.
6. If the answer has durable value, propose a synthesis page with its path, outline, sources, links, index entry, and log title.
7. Do not file the answer without explicit approval.
8. Append a `query` log entry only after the query operation is complete. Record `Applied: none` when nothing was filed.

If the index contains no adequate evidence, say so directly and identify the source or research needed. Do not infer an answer merely from file names or general model knowledge.

## File a synthesis

1. Place it in `wiki/syntheses/` with `type: synthesis`.
2. Cite all substantive inputs at claim level.
3. Link relevant concepts, technologies, patterns, and projects.
4. Label recommendations and inferences distinctly from source claims.
5. Set `evolving` when important evidence is missing or disputed.
6. Update `wiki/index.md` and record the filing in the query log entry.

## Lint

### Inspect

Check the requested scope for:

- pages missing from the index or index entries missing pages;
- invalid or incomplete frontmatter;
- broken, ambiguous, missing, or one-sided useful wikilinks;
- orphan pages with no inbound links;
- factual claims without nearby source-note citations;
- source notes without valid raw snapshots or origin metadata;
- stale or superseded claims not marked as such;
- contradictions hidden across pages;
- important recurring concepts without their own pages;
- log headings or fields that violate the append-only format.

### Propose and repair

1. Report findings by severity with exact paths and evidence.
2. Propose repairs, affected index entries, and the lint log title.
3. Stop and request approval before editing.
4. Apply only approved repairs without rewriting historical log entries.
5. Append one `lint` log entry recording findings, applied repairs, gaps, and approval state.

## Approval rules

- Accept clear approval of named paths or the entire presented change set.
- Treat altered scope as partial approval and apply only the overlap.
- Treat silence, a follow-up question, or approval of the summary as no write approval.
- Re-propose any material change not present in the approved set.
