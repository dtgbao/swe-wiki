---
title: "AWS IAM security baseline"
kind: practice
domain: aws/iam
status: draft
tags: [swe, aws, iam, security, identity, federation, identity-center, mfa, root-user, roles, temporary-credentials, least-privilege, access-review, shared-responsibility, governance]
sources: ["sources/2026-07-28-iam-best-practices.md", "sources/2026-07-28-shared-responsibility-model-for-iam.md", "https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-best-practices.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/gs-identities-mfa.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords_account-policy.html", "https://docs.aws.amazon.com/sdkref/latest/guide/access-temp-idc.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-enable-root-access.html", "https://aws.amazon.com/compliance/shared-responsibility-model/", "https://docs.aws.amazon.com/IAM/latest/UserGuide/id-credentials-access-keys-update.html"]
updated: 2026-07-28
confidence: high
---

# AWS IAM security baseline

Protect root access, use federated and temporary credentials by default, enforce MFA, scope role and policy permissions, and review identities and credentials continuously.

## Production priority order

1. Secure the AWS account root user and reserve it for tasks that require root
   credentials.
2. Give workforce users distinct federated identities, commonly through IAM
   Identity Center, and issue temporary role credentials.
3. Give workloads distinct IAM roles and automatically refreshed temporary
   credentials.
4. Apply least privilege to permission policies, role trust, role passing,
   resource policies, and session context.
5. Use an IAM user or long-lived access key only for a documented case that
   cannot use federation or a role.
6. Require MFA, prefer phishing-resistant factors, and protect identity and
   account recovery paths.
7. Review identities, credentials, permissions, and activity on a defined
   cadence; remove access through a controlled and reversible process.

## Shared-responsibility boundary

AWS secures and operates the IAM service and its underlying infrastructure.
The customer remains accountable for identity architecture, permission
requirements, configuration, credential protection, monitoring, remediation,
and customer-side compliance.

| AWS operates and supplies | Customer configures and governs |
| --- | --- |
| IAM service software, infrastructure, availability, and service-side vulnerability remediation | Root access, federation, users, groups, roles, policies, trust, role passing, boundaries, sessions, and guardrails |
| Authentication and authorization enforcement according to documented IAM behavior | Required access, least privilege, separation of duties, MFA coverage, recovery, and credential lifecycle |
| Managed capabilities, selected managed artifacts, security telemetry, and AWS-side compliance evidence | Whether managed artifacts fit, which telemetry to retain, how findings are reviewed, which changes are approved, and how customer controls are evidenced |

Configuration, vulnerability management, monitoring, and compliance can be
shared controls with separate AWS and customer layers. Assign a customer owner
to every required outcome even when an AWS feature automates part of the
control.

## Account and identity terminology

An **AWS account** is the security and resource boundary. Its **root user** is
the account identity created with the account. IAM users, IAM roles, federated
principals, and role sessions are identities or principals that access that
account; creating an IAM user does not create another AWS account.

Keep these terms precise in procedures and incident reports. Confusing an
account with an identity can produce incorrect ownership, recovery, and blast
radius assumptions.

## Course checklist mapped to current practice

| Introductory rule | Current production baseline |
| --- | --- |
| Use root only for account setup | Use root credentials only for tasks that require them. Enable MFA, avoid root access keys, secure the root email and phone, and alert on root use. Organizations can centrally secure member-account root access and remove member root credentials. |
| One IAM user per person | Never share identities, but prefer federation and temporary credentials for people. Create a separate IAM user only where federation is not available. |
| Assign users to groups and permissions to groups | Use groups for shared IAM-user permissions. For federated workforce access, use the identity provider or IAM Identity Center groups, permission sets, and account assignments. |
| Create a strong password policy | Configure it when IAM-user console passwords exist. It does not apply to the root password, access keys, or external identity-provider passwords. |
| Enforce MFA | All AWS account types require root-user MFA under current AWS guidance. Customers still own workforce coverage, identity-provider configuration, recovery, exceptions, and privileged-action policy. Prefer phishing-resistant factors where supported. |
| Use roles for AWS services | Give workloads roles rather than embedded user keys. Review the role trust policy, the caller's `iam:PassRole` scope, role permissions, and every workload sharing the role. |
| Generate access keys for CLI or SDK access | Let the CLI or SDK provider chain obtain temporary credentials from Identity Center, federation, or a workload role. Create long-lived IAM-user keys only as a documented fallback. |
| Rotate all keys often | Eliminate unnecessary long-lived keys first. Update remaining IAM-user access keys when needed and according to documented risk or compliance policy; temporary credentials expire and refresh automatically, while other key types need separate lifecycle rules. |
| Audit with the credential report and Access Advisor | Use the credential report for selected IAM credential status and IAM Last Accessed for review candidates. Validate changes with owners, workload schedules, policy context, and CloudTrail. |
| Never share users or access keys | Preserve this invariant for every identity and credential type. A colleague, application, and environment each need accountable access of their own. |

## Root-user controls

- Use the root user only when an operation explicitly requires root
  credentials.
- Register MFA and prefer phishing-resistant factors where supported.
- Do not create root access keys; investigate any existing root key.
- Protect the root email account and telephone number, and keep recovery
  details current.
- Monitor and alert on root-user sign-in and API activity.
- For an AWS Organization, evaluate centralized root-access management for
  member accounts, including removal of member root credentials and controlled
  privileged sessions.
- Document the recovery and emergency-use procedure and test the surrounding
  controls without normalizing routine root use.

## Human and workload access

| Actor | Default access pattern | Avoid |
| --- | --- | --- |
| Workforce human | Federated sign-in, preferably IAM Identity Center, MFA, and temporary role credentials | Shared identities, routine root use, and standing IAM-user keys |
| AWS-hosted workload | Service-supported IAM role and automatically refreshed temporary credentials | Embedded IAM-user keys or copied role credentials |
| External workload | Supported federation or temporary role-credential provider | Unowned, broadly privileged, long-lived keys |
| Exceptional IAM user | One accountable owner, minimum permissions, MFA for console access, and tightly controlled credentials | Group accounts, shared secrets, and undocumented exceptions |

An IAM-user password and IAM-user access key are independent credentials. An
IAM user can have either, both, or neither. A temporary credential set includes
an access key ID, secret access key, session token, and expiration; the
presence of an access-key ID therefore does not imply a long-lived IAM-user
credential.

## Operational checklist

### Establish

- [ ] Record the owner and purpose of every account, identity, role, and
  exceptional long-lived credential.
- [ ] Map required IAM controls to AWS-operated, customer-operated, and shared
  layers, with an accountable customer owner and evidence source.
- [ ] Secure root credentials, recovery channels, and root-use alerts.
- [ ] Configure federated workforce access and require MFA.
- [ ] Define workload roles with narrow trust and permission policies.
- [ ] Restrict `iam:PassRole` to approved roles and intended services.
- [ ] Apply least privilege to actions, resources, and conditions.

### Review

- [ ] Reconcile active people and workloads with their current access.
- [ ] Inspect credential reports for IAM-user password, MFA, and key findings.
- [ ] Verify that remaining long-lived access keys have documented update
  triggers and any required cadence.
- [ ] Review last-accessed data as a candidate signal, not a deletion rule.
- [ ] Validate findings with owners, architecture, schedules, and CloudTrail.
- [ ] Review federation, Identity Center, roles, service-specific credentials,
  and organization policies outside the credential report.
- [ ] Expire exceptions and remove stale credentials or permissions through a
  tested, reversible change.

### Respond

- [ ] Deactivate suspected exposed credentials promptly.
- [ ] Investigate recent activity and affected resources.
- [ ] Replace a long-lived credential only if the use case still requires it.
- [ ] Fix the source, build, storage, logging, or sharing path that exposed it.
- [ ] Reassess whether federation or a role can eliminate the credential.

## Related

- [AWS shared responsibility model for IAM](aws-shared-responsibility-model-for-iam.md)
- [AWS Identity and Access Management (IAM)](aws-identity-and-access-management.md)
- [Least-privilege access with AWS IAM](least-privilege-access-with-aws-iam.md)
- [AWS programmatic credential handling](aws-programmatic-credential-handling.md)
- [Reviewing AWS IAM credentials and permissions](reviewing-aws-iam-credentials-and-permissions.md)
- [AWS IAM roles for services](aws-iam-roles-for-services.md)
- [IAM Best Practices source](sources/2026-07-28-iam-best-practices.md)
- [Shared Responsibility Model for IAM source](sources/2026-07-28-shared-responsibility-model-for-iam.md)
- [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)
- [AWS IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [AWS root-user best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-best-practices.html)
- [AWS MFA guidance](https://docs.aws.amazon.com/IAM/latest/UserGuide/gs-identities-mfa.html)
- [AWS IAM password-policy scope](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords_account-policy.html)
- [AWS IAM Identity Center temporary credentials](https://docs.aws.amazon.com/sdkref/latest/guide/access-temp-idc.html)
- [AWS centralized root access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-enable-root-access.html)
- [AWS access-key update guidance](https://docs.aws.amazon.com/IAM/latest/UserGuide/id-credentials-access-keys-update.html)
