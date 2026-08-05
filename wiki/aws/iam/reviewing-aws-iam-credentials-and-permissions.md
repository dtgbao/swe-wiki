---
title: "Reviewing AWS IAM credentials and permissions"
kind: practice
domain: aws/iam
status: draft
tags: [swe, aws, iam, security, credential-report, last-accessed, access-review, least-privilege, audit, shared-responsibility, monitoring]
sources: ["sources/2026-07-28-iam-security-tools.md", "sources/2026-07-28-iam-best-practices.md", "sources/2026-07-28-shared-responsibility-model-for-iam.md", "https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_last-accessed.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_last-accessed-view-data.html", "https://docs.aws.amazon.com/IAM/latest/APIReference/API_GenerateServiceLastAccessedDetails.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-concepts.html", "https://aws.amazon.com/compliance/shared-responsibility-model/"]
updated: 2026-07-28
confidence: high
---

# Reviewing AWS IAM credentials and permissions

Use IAM credential inventory and last-accessed evidence together to find review candidates, then validate business need and audit history before changing credentials or permissions.

## Ownership boundary

AWS operates IAM and provides credential reports, Last Accessed information,
Access Analyzer findings, policy evaluation, and service telemetry. The
customer owns the control objective: defining review scope and cadence,
collecting required evidence, assigning findings, interpreting limitations,
approving access changes, monitoring rollout, and verifying remediation.

An AWS-generated finding or report does not transfer that accountability and
does not prove that a credential or permission is safe to remove.

## Two evidence surfaces

| Evidence | Scope | Useful for | Does not establish |
| --- | --- | --- | --- |
| IAM credential report | One account's root row and IAM users; IAM-managed passwords, first two access keys, MFA devices, and X.509 certificates | Credential presence, status, age, rotation, last-use, and MFA triage | Complete identity or credential inventory, effective permissions, or proof a credential can be safely removed |
| IAM last-accessed information | Users, groups, roles, and managed policies; allowed services and supported action data within the tracking period | Finding apparently unused service or action permissions and current granting policies | Successful access, complete API history, complete policy evaluation, or proof an absent event means unused access |

Credential reports are account snapshots. They exclude roles, federated and
IAM Identity Center identities, service-specific credentials, and other
credential systems. AWS stores one report per account and generates a fresh
report no more than once every four hours.

Last-accessed information reports authenticated attempts, including denied
requests. Recent events generally appear within four hours. Service-level
tracking generally covers the last 400 days, while action-level coverage and
regional start dates vary.

## Credential-review workflow

1. Generate and securely retain the account credential-report CSV with its
   generation timestamp.
2. Review the root row first: routine password or access-key use and missing
   MFA require immediate explanation.
3. For each IAM user, inspect password status and age, MFA status, access-key
   activity and rotation, and last-use fields.
4. Classify findings: expected and active, stale candidate, policy exception,
   break-glass credential, or unexplained.
5. Validate each candidate with the identity owner, workload owner, and
   available audit history.
6. Disable one candidate credential at a time when possible, monitor for
   impact, then delete it after the rollback window.
7. Record the decision, approver, evidence window, and follow-up date.

Treat the CSV as security-sensitive inventory even though it does not contain
secret access keys or passwords.

## Permission-refinement workflow

1. Select the user, group, role, or policy under review.
2. Enumerate services allowed by its identity permission policies.
3. Review service-level and available action-level last-accessed timestamps.
4. Identify the current policies that grant access to each candidate service.
5. Confirm the tracking period, supported action coverage, and whether the event
   represents an attempt rather than success.
6. Corroborate with workload owners, architecture, deployment schedules, and
   CloudTrail before removing access.
7. Make the smallest policy reduction, test both required success and expected
   denial, and retain a rollback path.
8. Monitor after rollout and repeat the review on a defined cadence.

Absence of last-accessed data is evidence to investigate, not a deletion rule.
Seasonal jobs, disaster-recovery paths, infrequent administrative operations,
and newly deployed workloads can be legitimate despite sparse history.

## Important limitations

- Last-accessed data includes denied attempts; CloudTrail is authoritative for
  whether API calls succeeded or failed.
- The allowed-service calculation uses identity permission policies and omits
  resource policies, ACLs, AWS Organizations policies, permissions boundaries,
  and session policies.
- `iam:PassRole` is not included in action last-accessed information.
- Action-level data covers supported management actions, not every data-plane
  operation.
- Current granting-policy information does not prove which policy authorized a
  historical request.
- “Not accessed in the tracking period” is bounded by the available service,
  action, and Region tracking window.
- Password last use and access-key last use measure different access paths; one
  cannot substitute for the other.

## Access Advisor versus Access Analyzer

The course calls the per-identity console view **IAM Access Advisor**. Current
IAM documentation exposes this as **Last Accessed** information.

**IAM Access Analyzer** is a separate capability that generates findings for
external, internal, or unused access. An unused-access analyzer can continuously
review users, roles, passwords, keys, and permissions, but it does not remove
the need for owner validation and controlled remediation.

## Review checks

- Does the credential report cover every AWS account in scope?
- Are root credentials protected and free of unexplained routine use?
- Are IAM-user MFA, password, and access-key findings assigned to an owner?
- Have excluded credential and identity systems been reviewed separately?
- Is the last-accessed reporting and action-coverage window understood?
- Were denied attempts distinguished from successful calls?
- Was CloudTrail and business context checked before removal?
- Are policy changes narrow, tested, reversible, and documented?
- Is there a recurring review cadence and exception-expiration process?

## Related

- [AWS shared responsibility model for IAM](aws-shared-responsibility-model-for-iam.md)
- [AWS IAM security baseline](aws-iam-security-baseline.md)
- [AWS Identity and Access Management (IAM)](aws-identity-and-access-management.md)
- [AWS programmatic credential handling](aws-programmatic-credential-handling.md)
- [Least-privilege access with AWS IAM](least-privilege-access-with-aws-iam.md)
- [IAM Security Tools source](sources/2026-07-28-iam-security-tools.md)
- [IAM Best Practices source](sources/2026-07-28-iam-best-practices.md)
- [Shared Responsibility Model for IAM source](sources/2026-07-28-shared-responsibility-model-for-iam.md)
- [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)
- [AWS credential-report documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html)
- [AWS last-accessed guidance](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_last-accessed.html)
- [Viewing IAM last-accessed information](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_last-accessed-view-data.html)
- [IAM Access Analyzer concepts](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-concepts.html)
