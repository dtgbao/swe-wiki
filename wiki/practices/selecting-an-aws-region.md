---
title: "Selecting an AWS Region"
kind: practice
status: draft
tags: [swe, aws, cloud, regions, compliance, latency, pricing]
sources: ["../sources/2026-07-28-ultimate-aws-certified-developer-associate-2026-dva-c02-aws-cloud-overview-regio.md"]
updated: 2026-07-28
confidence: medium
---

# Selecting an AWS Region

Choose an AWS Region by applying compliance and data residency, required service availability, user latency, and regional pricing.

## Decision inputs

| Factor | Question to answer | Evidence to collect |
| --- | --- | --- |
| Compliance and data residency | Where may the workload's data and processing legally reside? | Applicable legal, regulatory, contractual, and organizational locality requirements |
| Latency | Where are the primary users, and how close must the workload be to meet its response-time needs? | User geography and representative network-latency measurements |
| Service availability | Does the Region offer every required AWS service and feature? | Current AWS regional service and feature availability |
| Pricing | What does the expected service mix cost in each viable Region? | Current regional pricing for the workload's actual resource profile |

These are the four factors taught in the source. A production decision may have
additional workload-specific constraints, but they should not be invented
without requirements.

## Decision sequence

1. Eliminate Regions that violate compliance or data-residency constraints.
2. Eliminate Regions without required services or features.
3. Compare the remaining Regions against the latency needs of the primary user
   population.
4. Compare current regional pricing for the expected resource profile.
5. Record the selected Region, decisive evidence, rejected alternatives, and
   the conditions that require re-evaluation.

This ordering treats legal and technical feasibility as gates before optimizing
latency and cost. If the workload assigns different priorities, document that
tradeoff explicitly.

## Tradeoffs

- The Region nearest to users may not satisfy data-residency rules or offer a
  required service.
- The least expensive Region may impose unacceptable latency or omit a required
  feature.
- A residency constraint can deliberately narrow the choice to one country or
  jurisdiction even when another Region performs better.
- Choosing one Region establishes a deployment boundary; using a second Region
  is a separate architecture decision involving resource placement, state, and
  failover.

## Revalidation triggers

Revisit the decision when regulations or contracts change, the main user
population shifts, a required service or feature changes regional
availability, or regional prices materially change.

Do not maintain an unversioned static table of service availability or prices
as the source of truth. Verify both at decision and deployment time.

## Related

- [AWS global infrastructure and service scope](../systems/aws-global-infrastructure.md)
- [Source transcript](../sources/2026-07-28-ultimate-aws-certified-developer-associate-2026-dva-c02-aws-cloud-overview-regio.md)
