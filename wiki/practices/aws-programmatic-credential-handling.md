---
title: "AWS programmatic credential handling"
kind: practice
status: draft
tags: [swe, aws, iam, roles, credentials, access-keys, security, temporary-credentials, credential-report, security-review, shared-responsibility, credential-lifecycle]
sources: ["../sources/2026-07-28-aws-access-keys-cli-and-sdk.md", "../sources/2026-07-28-iam-roles-for-aws-services.md", "../sources/2026-07-28-iam-security-tools.md", "../sources/2026-07-28-iam-best-practices.md", "../sources/2026-07-28-shared-responsibility-model-for-iam.md", "https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html", "https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-authentication.html", "https://docs.aws.amazon.com/sdkref/latest/guide/standardized-credentials.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/securing_access-keys.html", "https://docs.aws.amazon.com/cli/latest/reference/iam/create-access-key.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_last-accessed.html", "https://docs.aws.amazon.com/IAM/latest/UserGuide/id-credentials-access-keys-update.html"]
updated: 2026-07-28
confidence: high
---

# AWS programmatic credential handling

Prefer automatically refreshed temporary AWS credentials and create long-lived IAM-user access keys only when the environment cannot use federation or roles.

## Credential selection order

1. For human users, use federation and temporary credentials, preferably
   through IAM Identity Center.
2. For workloads on AWS, attach an IAM role and let the SDK or CLI obtain
   temporary credentials from the runtime environment.
3. For external workloads, prefer temporary role credentials through a
   supported federation, web-identity, Roles Anywhere, or process provider.
4. Use a long-lived IAM-user access key only when no temporary-credential
   mechanism fits the use case.
5. Do not create root-user access keys for routine programmatic access.

This ordering reflects current AWS guidance. The course's IAM-user access-key
workflow is suitable as a credential-mechanics demonstration, but it is not the
preferred production default.

The summary lesson's statement that CLI or SDK users “must generate access
keys” describes request-signing mechanics too broadly. Temporary credentials
also contain access-key components, but the provider chain obtains and refreshes
them without creating a standing IAM-user access key.

## Credential forms

| Credential form | Components | Lifecycle |
| --- | --- | --- |
| Long-lived IAM-user access key | Access key ID and secret access key | Valid until manually deactivated or deleted |
| Temporary security credentials | Access key ID, secret access key, and session token | Expire after a limited session |

The access key ID identifies the credential. The secret access key is used to
sign requests and is shown only at creation. If the secret is lost, replace the
access key; it cannot be recovered.

## Workload role delivery

For an AWS-hosted workload, associate an IAM role through the service's
supported mechanism rather than distributing IAM-user credentials:

1. constrain which service may assume the role with its trust policy;
2. grant the role only the workload's required actions and resources;
3. restrict which deployment identities may pass that role to the service;
4. let the service deliver automatically refreshed temporary credentials;
5. let the SDK or CLI credential provider discover those credentials.

EC2 delivers role credentials through instance metadata via an instance
profile. Lambda assumes an execution role and makes temporary credentials
available to the function runtime. Other services have their own delivery
mechanisms, so verify the service-specific documentation rather than copying
the EC2 pattern literally.

Do not retrieve temporary credentials merely to persist, redistribute, or
inject them elsewhere. That defeats automatic rotation and expands the exposure
surface.

## Handling rules

- Give every human or workload its own identity; never share one credential
  pair between colleagues, applications, or environments.
- Never hardcode credentials in application source, scripts, mobile
  applications, container images, or infrastructure definitions.
- Never commit credentials to a repository or expose them in logs, screenshots,
  tickets, chat, or shell history.
- Let the CLI or SDK credential provider chain obtain and refresh credentials
  instead of passing secrets through application code.
- Store unavoidable long-lived keys only in an approved secure credential
  store, and limit the associated IAM permissions.
- Review last-used information and remove credentials that are unused or no
  longer required.
- Update or replace long-lived keys when personnel, workload ownership, or
  exposure risk changes.

## Rotation boundary

The shared-responsibility lesson says to rotate all keys often. For IAM access
keys, use a more precise lifecycle:

1. eliminate the long-lived access key when federation, a role, or another
   temporary provider can replace it;
2. remove an unused key instead of rotating it indefinitely;
3. update a remaining IAM-user access key when needed, including suspected
   compromise, personnel or ownership change, and any documented risk or
   compliance schedule;
4. create the replacement, update consumers, review last use, deactivate the
   old key, validate the new path, keep a short rollback window, and then
   delete the old key.

Temporary credentials expire and refresh automatically; they do not use this
manual rotation workflow. KMS keys, SSH keys, signing keys, and other
credentials also require their own lifecycle policy.

## Periodic credential inventory

Generate an IAM credential report for every account on a defined cadence:

- review the root row for MFA and unexplained password or access-key use;
- review IAM-user password, MFA, access-key status, rotation, and last-use
  fields;
- assign every stale, inactive, old, or unexpected credential to an owner for
  validation;
- review excluded identity and credential systems separately, including roles,
  federation, IAM Identity Center, and service-specific credentials;
- corroborate last-use metadata with business context and CloudTrail before
  deactivation or deletion.

The report is a cached account snapshot and can be newly generated at most once
every four hours. It identifies review candidates; it does not prove a
credential is safe to remove.

## Exposure response

If a credential may have been exposed:

1. Deactivate or delete it promptly.
2. Replace it only if the use case still requires a long-lived key.
3. Review access-key last-used data and relevant audit logs for misuse.
4. Correct the storage, distribution, or logging path that caused exposure.
5. Reassess whether a temporary credential provider can eliminate the key.

## Review checks

- Is this identity human or workload, and does the selected provider match?
- Can Identity Center, federation, an IAM role, or another temporary provider
  replace a long-lived key?
- Is the credential provider chain used instead of hardcoded secrets?
- Does each credential map to one accountable identity and environment?
- Are permissions least-privileged?
- Are any root-user keys present?
- Are unused or exposed credentials deactivated and removed?
- Do remaining long-lived keys have explicit update triggers, any required
  cadence, an owner, and a tested replacement procedure?
- Is the secret absent from source control, artifacts, logs, and messages?

## Related

- [AWS shared responsibility model for IAM](../concepts/aws-shared-responsibility-model-for-iam.md)
- [AWS IAM security baseline](aws-iam-security-baseline.md)
- [AWS programmatic access: CLI and SDKs](../systems/aws-programmatic-access-cli-and-sdks.md)
- [AWS Identity and Access Management (IAM)](../systems/aws-identity-and-access-management.md)
- [AWS IAM roles for services](../concepts/aws-iam-roles-for-services.md)
- [Reviewing AWS IAM credentials and permissions](reviewing-aws-iam-credentials-and-permissions.md)
- [Least-privilege access with AWS IAM](least-privilege-access-with-aws-iam.md)
- [Source lesson](../sources/2026-07-28-aws-access-keys-cli-and-sdk.md)
- [IAM roles for AWS services source](../sources/2026-07-28-iam-roles-for-aws-services.md)
- [IAM Security Tools source](../sources/2026-07-28-iam-security-tools.md)
- [IAM Best Practices source](../sources/2026-07-28-iam-best-practices.md)
- [Shared Responsibility Model for IAM source](../sources/2026-07-28-shared-responsibility-model-for-iam.md)
- [AWS IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [AWS CLI authentication options](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-authentication.html)
- [AWS secure access-key guidance](https://docs.aws.amazon.com/IAM/latest/UserGuide/securing_access-keys.html)
- [AWS credential-report documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html)
- [AWS last-accessed guidance](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_last-accessed.html)
- [AWS access-key update guidance](https://docs.aws.amazon.com/IAM/latest/UserGuide/id-credentials-access-keys-update.html)
