---
title: "Least-privilege access with AWS IAM"
kind: practice
domain: aws/iam
status: draft
tags: [swe, aws, iam, roles, service-roles, security, access-control, least-privilege, policy-evaluation, passrole, last-accessed, access-review, federation, identity-center, mfa, best-practices]
sources: ["sources/2026-07-28-iam-introduction-users-groups-policies.md", "sources/2026-07-28-iam-policies.md", "sources/2026-07-28-iam-roles-for-aws-services.md", "sources/2026-07-28-iam-security-tools.md", "sources/2026-07-28-iam-best-practices.md", "https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_last-accessed.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_last-accessed-view-data.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html"]
updated: 2026-07-28
confidence: high
---

# Least-privilege access with AWS IAM

Grant each AWS identity only the permissions needed for its responsibilities, and constrain both who may assume or pass a role and what its sessions may do.

## Core rules

- Use the AWS account root user only for tasks requiring root credentials,
  protect it with MFA, and do not share it.
- Give each person a distinct, non-shared identity; prefer federation and
  temporary credentials over an IAM user.
- Organize common responsibilities into groups; groups cannot nest.
- Use multiple group memberships for overlapping responsibilities.
- Grant only the services and actions needed for the work.
- Prefer a reusable managed policy when the same permissions apply to multiple
  identities; reserve inline policies for a strict one-to-one requirement.
- Treat ungrouped users as valid but exceptional because the source identifies
  that state as not best practice.
- For a service role, scope the trust policy to the intended service principal
  and scope the permission policies to the workload's required calls.
- Grant `iam:PassRole` only for approved role resources and services; broad
  pass-role authorization can let a deployer select a more privileged runtime
  identity.

The introductory users lesson teaches IAM users for people. Current AWS
guidance instead prefers federation, commonly through IAM Identity Center, for
workforce access. The user/group steps below are the fallback path for a case
that cannot use federation.

## IAM-user fallback access-design sequence

1. Document why federation or IAM Identity Center cannot satisfy the use case.
2. Identify the person's actual job responsibilities.
3. Map shared responsibilities to user groups.
4. Add the user to each applicable group rather than duplicating common access.
5. Define policies with only the required service actions.
6. Attach shared managed policies to the relevant groups.
7. Use an inline policy only when its lifecycle should remain one-to-one with a
   single user, group, or role.
8. Require MFA for console access and review the aggregate access from all
   group, direct, inline, and applicable resource-based policies.

Steps 4 and 6 are operational inferences from the course's group model. The
managed-versus-inline choice and aggregate evaluation are corroborated by
current AWS IAM documentation.

## Service-role design sequence

1. Identify the AWS service and the workload actions performed on the account's
   behalf.
2. Create a role whose trust policy permits only the intended service principal
   and required conditions.
3. Attach permission policies limited to the required actions, resources, and
   conditions.
4. Allow deployment identities to pass only the approved role to the intended
   service.
5. Associate the role through the service-specific mechanism, such as an EC2
   instance profile or Lambda execution-role setting.
6. Use the runtime's automatically refreshed temporary credentials through the
   standard SDK or CLI provider chain.
7. Audit role assumption and resulting API activity, then reduce permissions
   that are not required.

Trust, pass-role authorization, and runtime permissions are separate review
surfaces. Passing a role during configuration does not mean the deployer
assumes it; the AWS service assumes it when performing the delegated work.

## Evidence-driven permission review

1. Select the user, group, role, or managed policy to review.
2. Inspect services allowed by its identity permission policies.
3. Review service-level and supported action-level last-accessed data.
4. Identify current policies granting each candidate service permission.
5. Validate apparently unused access with the owner, workload schedule,
   architecture, and CloudTrail.
6. Remove or narrow the smallest permission set, test required success and
   expected denial, then monitor and retain a rollback path.

The course calls this console surface Access Advisor and demonstrates it for a
user. Current IAM last-accessed information also supports groups, roles, and
managed policies.

Interpret the evidence conservatively: timestamps represent attempts, including
denied requests; recent activity can lag; action and Region coverage varies;
several policy types are excluded from the allowed-service calculation; and
`iam:PassRole` is not tracked at action level. No recorded access is a review
signal, not sufficient evidence to delete a permission.

## Policy placement

| Placement | Use when | Review concern |
| --- | --- | --- |
| Managed policy on a group | A job function is shared by several users | Every current and future group member receives the policy |
| Managed policy directly on an identity | A reusable policy must attach without group indirection | Direct attachments can obscure role-based organization |
| Inline policy | The policy must have a strict one-to-one lifecycle with one user, group, or role | It is deleted with the identity and cannot be shared as one policy |
| Resource-based policy | A supported resource controls which principals may access it | `Principal`, cross-account scope, conditions, and interaction with identity policies |
| Role trust policy | An IAM role controls which principals may assume it | Service principal, account scope, conditions, and the permitted STS action |
| Permission policy on a service role | A service's role session needs bounded AWS API access | Workload blast radius across every resource using the shared role |

## Why restriction matters

Overly broad permissions create two failure classes named by the source:

- **Security exposure:** an identity can perform actions outside its legitimate
  responsibilities.
- **Cost exposure:** an identity can launch unnecessary AWS resources and incur
  charges.

The course's example policy illustrates action-level restriction. It allows
selected describe, list, and get operations for EC2, Elastic Load Balancing,
and CloudWatch rather than granting every operation on those services. Its
`"Resource": "*"` values are part of the example, not evidence that wildcard
resources are appropriate for every policy.

## Review checks

- Is routine work performed without the root user?
- Does every person use a distinct identity rather than a shared identity?
- Does each group represent a recognizable shared responsibility?
- Are any users ungrouped without an explicit reason?
- Do multiple group memberships create more aggregate access than intended?
- Does each policy allow only required actions?
- Could an inline policy be a reusable managed policy instead?
- Does an identity-based policy correctly omit `Principal`?
- Does a resource-based policy's `Principal` express the intended account or
  identity scope?
- Does each service role trust only the intended service principal?
- Can deployment identities pass only approved roles to approved services?
- Are role permissions narrow enough for every workload sharing the role?
- Does the workload use temporary role credentials instead of embedded
  long-lived keys?
- Was apparently unused access validated against the reporting window,
  supported actions, business need, and CloudTrail?
- Is the proposed reduction narrow, tested for success and denial, reversible,
  and assigned an owner?
- Does any applicable explicit deny override the intended allow?

## Evidence boundary

The course material ingested so far names `Condition` and `Deny` but does not
teach condition operators or complete evaluation. The roles lesson establishes
the service-role purpose and three examples, but does not cover trust-policy
JSON, `iam:PassRole`, AWS STS sessions, instance profiles, credential delivery,
service-linked roles, or role testing. Federation, IAM Identity Center,
permission boundaries, session policies, service control policies, resource
control policies, and complete policy evaluation remain outside the course
evidence. The security-tools lesson introduces credential reports and
user-level Access Advisor, but it does not define safe-removal gates, review
cadence, automation, CloudTrail validation, or IAM Access Analyzer.

## Related

- [AWS IAM security baseline](aws-iam-security-baseline.md)
- [AWS Identity and Access Management (IAM)](aws-identity-and-access-management.md)
- [AWS IAM roles for services](aws-iam-roles-for-services.md)
- [Reviewing AWS IAM credentials and permissions](reviewing-aws-iam-credentials-and-permissions.md)
- [IAM introduction source](sources/2026-07-28-iam-introduction-users-groups-policies.md)
- [IAM policies source](sources/2026-07-28-iam-policies.md)
- [IAM roles for AWS services source](sources/2026-07-28-iam-roles-for-aws-services.md)
- [IAM Security Tools source](sources/2026-07-28-iam-security-tools.md)
- [IAM Best Practices source](sources/2026-07-28-iam-best-practices.md)
- [AWS managed and inline policy reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html)
- [AWS policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
- [AWS IAM role concepts](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
- [AWS `iam:PassRole` guidance](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html)
- [AWS last-accessed guidance](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_last-accessed.html)
- [AWS IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
