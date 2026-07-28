# Wiki Activity Log

Append completed ingest, query, and lint operations below. Do not rewrite or reorder existing entries.

## [2026-07-26 23:37] bootstrap | SWE wiki initialized
- Changed: raw/, wiki/sources/, wiki/concepts/, wiki/decisions/, wiki/blueprints/, wiki/practices/, wiki/conventions/, wiki/systems/, wiki/questions/, .gitignore, wiki/index.md

## [2026-07-27 10:13] ingest | Practical multi-agent orchestration in Codex
- Changed: wiki/sources/2026-07-27-practical-multi-agent-orchestration-in-codex.md, wiki/blueprints/multi-agent-orchestration.md, wiki/index.md
- Notes: Captured reasoning-tiered roles, coordination flow, context inheritance, delegation boundaries, and verification controls.
- Follow-ups: Revalidate Codex model labels, tool names, and default concurrency as the product evolves.

## [2026-07-27 10:14] lint | Post-ingest validation: Practical multi-agent orchestration in Codex
- Notes: Mechanical lint passed. Semantic review found no contradictions, stale claims, orphaned durable pages, missing cross-links, weak provenance, or missing architecture diagrams.
- Follow-ups: Revalidate product-specific model and concurrency details when newer Codex sources are ingested.

## [2026-07-27 13:24] lint | Root documentation validation
- Changed: README.md, AGENTS.md, wiki/log.md
- Notes: Documented the human entry point, knowledge model, repository layout, agent invariants, and workflow completion gates. Mechanical lint passed; semantic review found no contradictions or schema drift.

## [2026-07-28 11:20] ingest | Ultimate AWS Certified Developer Associate 2026 DVA-C02 — AWS Cloud Overview: Region & AZ
- Changed: raw/2026-07-28-aws-cloud-overview-region-and-availability-zones.txt, wiki/sources/2026-07-28-ultimate-aws-certified-developer-associate-2026-dva-c02-aws-cloud-overview-regio.md, wiki/systems/aws-global-infrastructure.md, wiki/practices/selecting-an-aws-region.md, wiki/index.md
- Notes: Captured AWS geographic boundaries, failure domains, service scope, and a four-factor Region-selection practice; isolated mutable course-era figures from durable guidance.
- Follow-ups: Add original video metadata and revalidate current AWS infrastructure counts, resource scope, service availability, and pricing against authoritative AWS documentation.

## [2026-07-28 11:21] lint | Post-ingest validation: AWS Cloud Overview - Region & AZ
- Changed: wiki/sources/2026-07-28-ultimate-aws-certified-developer-associate-2026-dva-c02-aws-cloud-overview-regio.md, wiki/index.md, wiki/log.md
- Notes: Mechanical lint passed. Semantic review found no contradictions, orphaned durable pages, broken provenance, missing cross-links, unbounded mutable claims, or missing architecture diagrams; corrected the generated source index summary.
- Follow-ups: Add original video metadata and revalidate current AWS infrastructure and service facts before production use.

## [2026-07-28 11:41] ingest | IAM Introduction: Users, Groups, Policies
- Changed: raw/2026-07-28-iam-introduction-users-groups-policies.txt, raw/2026-07-28-iam-users-and-groups-slide.png, raw/2026-07-28-iam-permissions-policy-slide.png, wiki/sources/2026-07-28-iam-introduction-users-groups-policies.md, wiki/systems/aws-identity-and-access-management.md, wiki/practices/least-privilege-access-with-aws-iam.md, wiki/systems/aws-global-infrastructure.md, wiki/index.md
- Notes: Captured IAM global scope, root-user separation, non-nested many-to-many user groups, the slide policy structure and actions, and least-privilege guidance.
- Follow-ups: Ingest later material on roles, policy evaluation, explicit deny, MFA, federation or IAM Identity Center, and validate current AWS human-access guidance.

## [2026-07-28 11:41] lint | Post-ingest validation: IAM Introduction: Users, Groups, Policies
- Changed: wiki/log.md
- Notes: Mechanical lint passed. Semantic review found no contradictions, orphaned durable pages, broken provenance, missing cross-links, vague index summaries, or uncaveated course-only security claims; the existing global-service page now cross-links IAM.
- Follow-ups: Add later authoritative material for modern human access, root protections, roles, policy evaluation, explicit deny, conditions, and policy testing.

## [2026-07-28 12:44] ingest | IAM Policies
- Changed: raw/2026-07-28-iam-policies.txt, raw/2026-07-28-iam-policy-inheritance-slide.png, raw/2026-07-28-iam-policy-structure-slide.png, wiki/sources/2026-07-28-iam-policies.md, wiki/systems/aws-identity-and-access-management.md, wiki/practices/least-privilege-access-with-aws-iam.md, wiki/index.md
- Notes: Captured group-policy inheritance, inline-policy attachment, JSON policy anatomy, and the S3 example; reconciled Principal scope and effective evaluation against current AWS IAM documentation.
- Follow-ups: Ingest later material on condition operators, roles, permission boundaries, session policies, SCPs, RCPs, Access Analyzer, and policy testing.

## [2026-07-28 12:44] lint | Post-ingest validation: IAM Policies
- Changed: wiki/log.md
- Notes: Mechanical lint passed. Semantic review found no orphaned pages, broken provenance, vague index summaries, or unresolved contradictions; the resource-based Principal example, account principal semantics, inline-policy scope, and explicit-deny precedence are recorded against current AWS documentation.
- Follow-ups: Add future course material for conditions, roles, boundaries, session policies, organization policies, Access Analyzer, and hands-on policy validation.

## [2026-07-28 14:20] ingest | AWS Access Keys, CLI and SDK
- Changed: raw/2026-07-28-aws-access-keys-cli-and-sdk.txt, raw/2026-07-28-aws-access-methods-slide.png, wiki/sources/2026-07-28-aws-access-keys-cli-and-sdk.md, wiki/systems/aws-programmatic-access-cli-and-sdks.md, wiki/practices/aws-programmatic-credential-handling.md, wiki/systems/aws-identity-and-access-management.md, wiki/index.md
- Notes: Captured console, CLI, and SDK boundaries plus access-key mechanics; reconciled the lesson with temporary-first AWS guidance, credential-provider chains, and the Botocore/Boto3 relationship.
- Follow-ups: Ingest the hands-on CLI setup and later material on Identity Center, roles, profiles, credential precedence, rotation, revocation, and exposure detection.

## [2026-07-28 14:22] lint | Post-ingest validation: AWS Access Keys, CLI and SDK
- Changed: wiki/log.md
- Notes: Mechanical lint passed. Semantic review found no orphaned pages, broken provenance, vague index summaries, or unresolved contradictions; the course IAM-user access-key workflow is bounded by current temporary-first AWS guidance and credential-provider-chain behavior.
- Follow-ups: Add the hands-on CLI setup and future material on Identity Center, roles, profiles, credential precedence, rotation, revocation, and exposure detection.

## [2026-07-28 14:43] ingest | IAM Roles for AWS Services
- Changed: raw/2026-07-28-iam-roles-for-aws-services.txt, raw/2026-07-28-iam-roles-for-services-slide.png, wiki/sources/2026-07-28-iam-roles-for-aws-services.md, wiki/concepts/aws-iam-roles-for-services.md, wiki/practices/aws-programmatic-credential-handling.md, wiki/practices/least-privilege-access-with-aws-iam.md, wiki/systems/aws-identity-and-access-management.md, wiki/systems/aws-programmatic-access-cli-and-sdks.md, wiki/index.md, wiki/log.md
- Notes: Preserved the transcript and slide; added the service-role trust, pass-role, permission, temporary-session, and service-specific delivery model while retaining the course-level EC2, Lambda, and CloudFormation examples.
- Follow-ups: Ingest the role-creation hands-on and later coverage of trust-policy JSON, STS sessions, instance profiles, service-linked roles, credential delivery, audit evidence, and denial-path testing.

## [2026-07-28 14:44] lint | Post-ingest validation: IAM Roles for AWS Services
- Changed: wiki/log.md
- Notes: Mechanical lint passed. Semantic review found no orphaned pages, broken provenance, vague index summaries, missing cross-links, or unresolved contradictions; the course user analogy is bounded by role trust, pass-role authorization, permission policies, temporary sessions, and service-specific delivery.
- Follow-ups: Validate the next hands-on against the durable model and extend it with trust-policy JSON, STS session behavior, instance profiles, service-linked roles, runtime credential delivery, audit evidence, and denial-path tests.

## [2026-07-28 15:45] ingest | IAM Security Tools
- Changed: raw/2026-07-28-iam-security-tools.txt, raw/2026-07-28-iam-security-tools-slide.png, wiki/sources/2026-07-28-iam-security-tools.md, wiki/practices/reviewing-aws-iam-credentials-and-permissions.md, wiki/practices/aws-programmatic-credential-handling.md, wiki/practices/least-privilege-access-with-aws-iam.md, wiki/systems/aws-identity-and-access-management.md, wiki/index.md, wiki/log.md
- Notes: Preserved the transcript and slide; added the credential-report and last-accessed review model, including entity scope, tracking and policy limitations, CloudTrail corroboration, and controlled least-privilege remediation.
- Follow-ups: Ingest later automation and define production review cadence, ownership, exception expiry, evidence retention, CloudTrail validation, IAM Access Analyzer usage, and tested rollback procedures.

## [2026-07-28 15:46] lint | Post-ingest validation: IAM Security Tools
- Changed: wiki/log.md
- Notes: Mechanical lint passed. Semantic review found no orphaned pages, broken provenance, vague index summaries, missing cross-links, or unresolved contradictions; credential-report and last-accessed findings are bounded by their entity, credential, tracking, policy, and event-semantics limitations and require owner and CloudTrail validation before remediation.
- Follow-ups: Extend the review practice with production cadence, ownership, exception expiry, evidence retention, IAM Access Analyzer integration, automation, test cases, rollout monitoring, and rollback evidence.

## [2026-07-28 16:00] ingest | IAM Best Practices
- Changed: raw/2026-07-28-iam-best-practices.txt, raw/2026-07-28-iam-guidelines-best-practices-slide.png, raw/2026-07-28-iam-section-summary-slide.png, wiki/sources/2026-07-28-iam-best-practices.md, wiki/practices/aws-iam-security-baseline.md, wiki/practices/aws-programmatic-credential-handling.md, wiki/practices/least-privilege-access-with-aws-iam.md, wiki/practices/reviewing-aws-iam-credentials-and-permissions.md, wiki/systems/aws-identity-and-access-management.md, wiki/index.md, wiki/log.md
- Notes: Preserved the transcript and two slides; consolidated the course checklist into a production IAM baseline and reconciled account-versus-identity terminology, root-only tasks, federated human access, MFA scope, service roles, temporary credentials, password-policy scope, and evidence-backed access review.
- Follow-ups: Validate future hands-on lessons against federation and IAM Identity Center, phishing-resistant MFA, centralized root access, temporary CLI and SDK credentials, CloudTrail, IAM Access Analyzer, and organization guardrails.

## [2026-07-28 16:01] lint | Post-ingest validation: IAM Best Practices
- Changed: wiki/log.md
- Notes: Mechanical lint passed. Semantic review found no orphaned durable pages, broken provenance, vague index summaries, missing cross-links, or unresolved contradictions; course-era claims about accounts, root use, IAM users, MFA, access keys, password policy, roles, and audit tools are explicitly bounded by current federation-first, temporary-credential, and evidence-backed production guidance.
- Follow-ups: Validate later course hands-on material against IAM Identity Center and federation, phishing-resistant MFA, centralized root access, temporary credential providers, CloudTrail, IAM Access Analyzer, and organization guardrails.

## [2026-07-28 16:12] ingest | Shared Responsibility Model for IAM
- Changed: raw/2026-07-28-shared-responsibility-model-for-iam.txt, raw/2026-07-28-shared-responsibility-model-for-iam-slide.png, wiki/sources/2026-07-28-shared-responsibility-model-for-iam.md, wiki/concepts/aws-shared-responsibility-model-for-iam.md, wiki/practices/aws-iam-security-baseline.md, wiki/practices/aws-programmatic-credential-handling.md, wiki/practices/reviewing-aws-iam-credentials-and-permissions.md, wiki/systems/aws-identity-and-access-management.md, wiki/index.md, wiki/log.md
- Notes: Preserved the transcript and slide; added an IAM shared-responsibility control matrix and reconciled infrastructure versus configuration, shared compliance and vulnerability controls, root and workforce MFA ownership, managed IAM artifacts, evidence review, and need-based long-lived access-key updates.
- Follow-ups: Map the model to later service-specific lessons, distinguish every credential and cryptographic key type, define control owners and evidence, and confirm whether the CCP exam reference is reused DVA-C02 material.

## [2026-07-28 16:13] lint | Post-ingest validation: Shared Responsibility Model for IAM
- Changed: wiki/log.md
- Notes: Mechanical lint passed. Semantic review found no unresolved contradictions, stale unbounded claims, orphaned durable pages, broken provenance, vague index summaries, missing cross-links, or missing useful diagrams; AWS-operated service controls and customer-owned IAM outcomes are separated, and compliance, vulnerability management, MFA, monitoring, and credential rotation are qualified by layer and credential type.
- Follow-ups: Apply the responsibility matrix to later AWS service lessons, add control ownership and evidence requirements, distinguish access keys from cryptographic and protocol keys, and resolve the transcript CCP-versus-DVA exam-context note.
