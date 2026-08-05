---
title: "Practical multi-agent orchestration in Codex"
kind: source
domain: software-engineering/ai-agents
status: draft
tags: [swe, multi-agent, codex, orchestration, delegation]
sources: ["https://x.com/pvncher/status/2080707291603407077"]
updated: 2026-07-27
confidence: high
---

# Practical multi-agent orchestration in Codex

## Provenance

- Source: https://x.com/pvncher/status/2080707291603407077
- Article: "Practical multi-agent orchestration in Codex"
- Author: Eric Provencher (`@pvncher`)
- Published: 2026-07-24
- Article ID: `2080691655238107136`
- Ingested: 2026-07-27

## Summary

Codex Multi-Agent V2 coordinates broad engineering work through reasoning-matched roles, explicit ownership, direct agent messaging, and centralized user approvals.

The recommended default is one model family with reasoning effort matched to
the assignment: lightweight scouts for narrow read-only discovery, medium
workers for scoped implementation, and high-reasoning workers for difficult or
ambiguous work. A primary coordinator assigns non-overlapping work, tracks
ownership, and remains available to the user while agents exchange findings
directly.

Context inheritance, delegation authority, and concurrency are independent
controls. Inheriting conversation history supplies goal and decision context,
while `fork_turns: "none"` gives an agent a focused fresh start. Fresh agents
must receive essential safety and tool boundaries explicitly; inherited agents
that should remain leaves need an explicit instruction not to spawn more
agents. The article recommends starting with these defaults and tuning them
empirically rather than maximizing reasoning on every task.

## SWE Extraction

### Architecture and control flow

- A coordinator owns decomposition, assignment, progress tracking, result
  integration, and user-facing approvals.
- Scouts investigate separate, narrow questions in parallel without changing
  state.
- Workers implement clearly owned changes and run the checks for their scope.
- Agents have separate inboxes on a shared messaging system, so a scout can
  send a dependency directly to the worker that needs it.
- Concurrency is a per-thread budget and includes the coordinator. The observed
  default in the article is four agents total.

### Role and reasoning defaults

- **Scout — GPT-5.6 Sol Light:** locate files, trace code paths, and identify
  tests for tightly bounded, read-only questions.
- **Worker — GPT-5.6 Sol Medium:** implement scoped changes, run checks, and
  complete routine supporting work.
- **Smart worker — GPT-5.6 Sol High:** handle difficult implementation,
  ambiguity, or local coordination when deeper reasoning is justified.
- Keep the model family constant first and tune reasoning effort; introduce a
  different model family only when the task benefits enough to justify the
  added coordination complexity.
- Ultra reasoning makes coordination more proactive but is best reserved for
  high-stakes work with substantial ambiguity or scattered context.

### Context and delegation boundaries

- Conversation-history forks preserve the broad goal and prior decisions.
- `fork_turns: "none"` creates a fresh, focused agent and reduces irrelevant
  inherited context.
- Fresh-context assignments must restate all task-specific safety, tool, and
  authority restrictions.
- An inherited-context agent may also inherit orchestration instructions. When
  it must remain a leaf, state that it should complete the assignment directly
  and must not spawn agents.
- Give every implementation agent exclusive ownership and avoid duplicate
  investigations.
- Keep consequential approvals with the user even when work is delegated.

### Operational practice

- Stay responsive to the user while substantive work runs behind the scenes.
- Parallelize only independent discovery or clearly partitioned implementation.
- Allow direct agent-to-agent handoffs when one result unblocks another.
- Experiment with reasoning effort, inherited context, delegation authority,
  and collaboration topology; judge settings by useful progress per reasoning
  cost.

## Impacted Pages

- [Multi-agent orchestration blueprint](../multi-agent-orchestration.md)
  — reusable roles, assignment protocol, context rules, and failure checks.

## Open Questions

- How do quality, latency, and token cost change across reasoning tiers on the
  same representative task set?
- Which task shapes benefit from direct agent-to-agent messaging versus routing
  every dependency through the coordinator?
- The named models, reasoning labels, tool names, and default concurrency are
  product-specific observations and should be revalidated as Codex evolves.
