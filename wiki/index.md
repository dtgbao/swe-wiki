# SWE Wiki Index

Content catalog. Read this before querying pages.

## Sources

- [AWS Access Keys, CLI and SDK](sources/2026-07-28-aws-access-keys-cli-and-sdk.md) - The lesson distinguishes AWS console, CLI, and SDK access, introduces IAM-user access key pairs, and explains CLI scripting and SDK embedding. | tags: swe, aws, iam, access-keys, cli, sdk, credentials, automation | updated: 2026-07-28 | sources: 2
- [EC2 Instance Types Basics](sources/2026-07-30-ec2-instance-types-basics.md) - The lesson introduces EC2 instance-type naming, workload-oriented categories, and the resource tradeoffs among general-purpose, compute-optimized, memory-optimized, and storage-optimized instances. | tags: swe, aws, ec2, instance-types, compute, memory, storage, sizing, performance, free-tier | updated: 2026-07-30 | sources: 6
- [IAM Best Practices](sources/2026-07-28-iam-best-practices.md) - The lesson consolidates root-user protection, distinct identities, group-based permissions, MFA, service roles, credential security, and access review into an introductory IAM checklist. | tags: swe, aws, iam, security, best-practices, root-user, federation, mfa, roles, access-keys, least-privilege | updated: 2026-07-28 | sources: 3
- [IAM Introduction: Users, Groups, Policies](sources/2026-07-28-iam-introduction-users-groups-policies.md) - The lesson introduces AWS IAM users, groups, JSON policies, and least privilege as the basic identity and permission model for an AWS account. | tags: swe, aws, iam, identity, access-control, policies, least-privilege | updated: 2026-07-28 | sources: 3
- [IAM Policies](sources/2026-07-28-iam-policies.md) - The lesson explains how IAM users receive policies through group membership or direct inline attachment and introduces the main elements of an AWS JSON policy document. | tags: swe, aws, iam, policies, access-control, inheritance, policy-language | updated: 2026-07-28 | sources: 3
- [IAM Roles for AWS Services](sources/2026-07-28-iam-roles-for-aws-services.md) - The lesson introduces IAM roles as the mechanism that lets AWS services perform authorized actions in an AWS account. | tags: swe, aws, iam, roles, service-roles, temporary-credentials, ec2, lambda, cloudformation | updated: 2026-07-28 | sources: 2
- [IAM Security Tools](sources/2026-07-28-iam-security-tools.md) - The lesson introduces the IAM credential report for account-level credential inventory and IAM Access Advisor for reviewing granted service permissions and last-accessed information. | tags: swe, aws, iam, security, credential-report, access-advisor, last-accessed, least-privilege, audit | updated: 2026-07-28 | sources: 2
- [Practical multi-agent orchestration in Codex](sources/2026-07-27-practical-multi-agent-orchestration-in-codex.md) - Codex Multi-Agent V2 coordinates broad engineering work through reasoning-matched roles, explicit ownership, direct agent messaging, and centralized user approvals. | tags: swe, multi-agent, codex, orchestration, delegation | updated: 2026-07-27 | sources: 1
- [Security Groups and Classic Ports Overview](sources/2026-07-30-security-groups-and-classic-ports-overview.md) - The lesson introduces security groups as allow-list controls for EC2 ingress and egress, explains CIDR and group references, gives connectivity troubleshooting heuristics, and catalogs common service ports. | tags: swe, aws, ec2, vpc, security-groups, network-security, firewall, stateful, ports, ssh, rdp, troubleshooting | updated: 2026-07-30 | sources: 7
- [Shared Responsibility Model for IAM](sources/2026-07-28-shared-responsibility-model-for-iam.md) - The lesson assigns AWS responsibility for the cloud infrastructure and customer responsibility for IAM identities, permissions, MFA, credentials, monitoring, and access review. | tags: swe, aws, iam, security, shared-responsibility, governance, mfa, access-keys, permissions, monitoring, compliance | updated: 2026-07-28 | sources: 2
- [Ultimate AWS Certified Developer Associate 2026 DVA-C02 — AWS Cloud Overview: Region & AZ](sources/2026-07-28-ultimate-aws-certified-developer-associate-2026-dva-c02-aws-cloud-overview-regio.md) - The lesson introduces AWS's geographic model: Regions, Availability Zones, data centers, and edge locations or points of presence. | tags: swe, aws, cloud, infrastructure, regions, availability-zones | updated: 2026-07-28 | sources: 1

## Concepts

- [AWS IAM roles for services](concepts/aws-iam-roles-for-services.md) - An IAM service role is an assumable AWS identity that lets a trusted AWS service perform bounded actions using temporary credentials. | tags: swe, aws, iam, roles, service-roles, trust-policy, temporary-credentials, passrole, ec2, lambda, cloudformation | updated: 2026-07-28 | sources: 7
- [AWS shared responsibility model for IAM](concepts/aws-shared-responsibility-model-for-iam.md) - AWS operates and secures the IAM service, while customers remain accountable for identity design, permission configuration, credential protection, monitoring, remediation, and customer-side compliance. | tags: swe, aws, iam, security, shared-responsibility, governance, identity, permissions, mfa, credentials, monitoring, compliance | updated: 2026-07-28 | sources: 7

## Decisions

_None yet._

## Blueprints

- [Multi-agent orchestration blueprint](blueprints/multi-agent-orchestration.md) - Use a coordinator and reasoning-matched agents to parallelize independent engineering work while preserving ownership, shared findings, and user control. | tags: swe, multi-agent, orchestration, delegation, codex | updated: 2026-07-27 | sources: 1

## Practices

- [AWS IAM security baseline](practices/aws-iam-security-baseline.md) - Protect root access, use federated and temporary credentials by default, enforce MFA, scope role and policy permissions, and review identities and credentials continuously. | tags: swe, aws, iam, security, identity, federation, identity-center, mfa, root-user, roles, temporary-credentials, least-privilege, access-review, shared-responsibility, governance | updated: 2026-07-28 | sources: 10
- [AWS programmatic credential handling](practices/aws-programmatic-credential-handling.md) - Prefer automatically refreshed temporary AWS credentials and create long-lived IAM-user access keys only when the environment cannot use federation or roles. | tags: swe, aws, iam, roles, credentials, access-keys, security, temporary-credentials, credential-report, security-review, shared-responsibility, credential-lifecycle | updated: 2026-07-28 | sources: 14
- [Least-privilege access with AWS IAM](practices/least-privilege-access-with-aws-iam.md) - Grant each AWS identity only the permissions needed for its responsibilities, and constrain both who may assume or pass a role and what its sessions may do. | tags: swe, aws, iam, roles, service-roles, security, access-control, least-privilege, policy-evaluation, passrole, last-accessed, access-review, federation, identity-center, mfa, best-practices | updated: 2026-07-28 | sources: 12
- [Reviewing AWS IAM credentials and permissions](practices/reviewing-aws-iam-credentials-and-permissions.md) - Use IAM credential inventory and last-accessed evidence together to find review candidates, then validate business need and audit history before changing credentials or permissions. | tags: swe, aws, iam, security, credential-report, last-accessed, access-review, least-privilege, audit, shared-responsibility, monitoring | updated: 2026-07-28 | sources: 9
- [Selecting an AWS Region](practices/selecting-an-aws-region.md) - Choose an AWS Region by applying compliance and data residency, required service availability, user latency, and regional pricing. | tags: swe, aws, cloud, regions, compliance, latency, pricing | updated: 2026-07-28 | sources: 1

## Conventions

_None yet._

## Systems

- [AWS Identity and Access Management (IAM)](systems/aws-identity-and-access-management.md) - AWS IAM is a global service that connects identities to permission policies and credentials used to authenticate AWS access. | tags: swe, aws, iam, identity, access-control, users, groups, roles, service-roles, policies, policy-language, policy-evaluation, credentials, access-keys, temporary-credentials, credential-report, last-accessed, security-review, root-user, federation, identity-center, mfa, password-policy, best-practices, shared-responsibility, governance | updated: 2026-07-28 | sources: 23
- [AWS global infrastructure and service scope](systems/aws-global-infrastructure.md) - AWS Regions scope most deployments, Availability Zones isolate failures within a Region, and edge locations move delivery closer to users. | tags: swe, aws, cloud, infrastructure, regions, availability-zones, edge | updated: 2026-07-28 | sources: 2
- [AWS programmatic access: CLI and SDKs](systems/aws-programmatic-access-cli-and-sdks.md) - The AWS CLI serves shell users and automation, while AWS SDKs embed service clients in applications; both authenticate through configurable credential providers. | tags: swe, aws, cli, sdk, api, credentials, automation, programmatic-access | updated: 2026-07-28 | sources: 7
- [Amazon EC2 instance types](systems/amazon-ec2-instance-types.md) - An Amazon EC2 instance type is a named hardware-resource configuration selected to match a workload's compute, memory, accelerator, storage, and network requirements. | tags: swe, aws, ec2, instance-types, compute, memory, storage, accelerators, hpc, sizing, performance, cost | updated: 2026-07-30 | sources: 10
- [Amazon VPC security groups](systems/amazon-vpc-security-groups.md) - An Amazon VPC security group is a stateful, allow-list network control associated with one or more network interfaces. | tags: swe, aws, ec2, vpc, security-groups, network-security, firewall, stateful, ingress, egress, ports, troubleshooting | updated: 2026-07-30 | sources: 12

## Questions

_None yet._
