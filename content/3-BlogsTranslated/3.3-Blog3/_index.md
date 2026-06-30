---
title: "Blog 3"
date: 2026-05-20
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Cyber resilience on AWS: A reference approach for recovery from ransomware and destructive events

## Problem statement: recover to a known-good state

Cyber resilience is the ability to recover workloads to a **known-good** state after an adversary has affected the environment. Prevention helps keep threat actors out and detection helps find them quickly, but cyber resilience focuses on **recovery** when backups, credentials, or parts of the infrastructure can’t be assumed to be safe.

This post summarizes a reference approach that:

- Isolates recovery from production so compromised identities don’t block recovery
- Protects backup storage from deletion through **AWS Backup logically air-gapped vaults**
- Validates recovery points before restore to reduce the risk of reintroducing the same threat
- Uses a practical decision model: **Rebuild–Restore–Rotate**

---

## Isolating recovery from production (separate trust boundaries)

The core idea is that the recovery environment (identities, keys, and network paths) shouldn’t share a trust boundary with the environment being recovered. A common pattern inside **AWS Organizations** uses three account roles:

- **Production account**: Runs workloads. After a confirmed event, it’s isolated for investigation. Recovery work doesn’t happen here because remediation in place may not fully restore trust.
- **Recovery account**: Owns the AWS Backup **logically air-gapped vault** and enforces deletion protection. It controls restore authorization and can require **Multi-party approval (MPA)**. SCPs can scope the account to backup-only operations.
- **Isolated Recovery Environment (IRE)**: Where backups are restored and validated, and where a new production environment is rebuilt before cutover. It has no trust relationship to production, no VPC peering to it, and no internet-facing resources, so a tainted restore stays contained.

![Account separation and trust boundaries](/images/3-BlogsTranslated/blog3/revblog-1142-images-1.png)

---

## Best practices for AWS Backup logically air-gapped vault

Key points to treat the vault as a recovery control plane:

- **Use the vault for deletion protection**: a logically air-gapped vault is locked in Compliance mode, so recovery points can’t be deleted or have retention shortened by any principal (including root) during the retention window.
- **Understand where recovery points live**: the vault object is in your Recovery account, but recovery points are stored in AWS service-owned accounts; encryption can use AWS-owned keys or AWS KMS customer managed keys.
- **Share recovery points for restore**: share across accounts via **AWS RAM** so the IRE can restore from the Recovery account vault.
- **Require MPA for restores**: approval through IAM Identity Center is useful when production accounts are not trusted.
- **Back up managed resources directly**: AWS Backup can write directly to the air-gapped vault for services like Amazon S3, Amazon DynamoDB, and Amazon EFS.
- **Cover S3 edge cases**: for data that can’t be backed up by AWS Backup (for example SSE-C objects or certain storage classes), use **S3 Object Lock (Compliance mode)** with **S3 Versioning**.

---

## Validation pipeline: “restorable” is not the same as “safe”

A successful restore only proves the backup was readable. Validation is about proving it’s **safe to use**. No single check catches everything, so validation should combine layers:

| Layer type | Capability | What it provides |
| --- | --- | --- |
| AWS native | AWS Backup Restore Testing | Automated restore verification, with custom hooks via PutRestoreValidationResult |
| AWS native | Amazon GuardDuty Malware Protection | Malware scanning on restored volumes |
| AWS Partner | Marketplace partner solutions | Content-level ransomware scanning without requiring full restore first |
| Workload-specific | Integrity and consistency checks | DB consistency, application invariants, configuration diffs vs known-good baselines |
| Cross-cutting | Log and audit review | Detect unexpected identity/config changes using CloudTrail and workload logs |

Validation should run inside the **IRE** so that any failed restore stays contained and can’t spread back to production or out to the internet.

---

## Selecting a safe recovery point

For cyber events, the most recent backup may be unsafe if the adversary was present before detection. A practical approach:

1. Build an investigation timeline (CloudTrail, VPC Flow Logs, GuardDuty, Security Hub, workload logs) to identify the earliest plausible indicator.
2. Treat that timestamp as the **event boundary**.
3. Evaluate recovery point candidates in reverse chronological order starting from the most recent backup that predates the boundary.
4. Run the validation pipeline for each candidate; if it fails, step back to an earlier recovery point.

![Selecting the most recent safe recovery point](/images/3-BlogsTranslated/blog3/figure2.png)

---

## Recovery workflow (parallelizable stages)

Recovery has five stages. The overall downtime is determined by the slowest path, so multiple stages should run in parallel:

1. **Establish the timeline** (investigation)
2. **Validate candidates** (validation pipeline)
3. **Approval** (gate before restore; often MPA)
4. **Rebuild and restore** (rebuild infra from IaC, then restore validated data, rotate credentials)
5. **Cutover** (shift traffic to rebuilt environment, update cross-account dependencies)

![Parallel recovery workflow and approval gate](/images/3-BlogsTranslated/blog3/Picture6.jpg)

---

## The Rebuild–Restore–Rotate framework

Cyber recovery requires sorting what should be rebuilt from code, restored from backup, and generated fresh:

**Infrastructure is code. Data is backup. Credentials are new.**

| Category | Examples | Why |
| --- | --- | --- |
| Rebuild from code | IAM roles/policies, Security Groups, VPC, EC2, Lambda, CI/CD definitions | Prefer reviewed, version-controlled IaC over potentially tainted config |
| Restore from backup | Aurora/EFS/EBS/FSx | Business data can’t be recreated and must come from validated immutable backups |
| Rotate or re-issue | Access keys, DB passwords, API keys, certificates, OAuth tokens, SSH keys | Replace secrets that may have been exposed during the event window |

Note that some resources span categories (for example, S3 and DynamoDB have both configuration and data). Derived stores (search indexes, caches, analytics tables) may be rebuilt from restored sources and should be sequenced accordingly in the runbook.

---

## Next steps (actionable checklist)

- Create a logically air-gapped vault in a dedicated Recovery account and configure MPA.
- Establish an IRE in advance with no trust relationship or network path to production; enforce isolation with SCPs.
- Schedule AWS Backup Restore Testing and enable GuardDuty Malware Protection for scanning.
- Define workload-specific integrity checks (DB consistency, invariants, config diffs).
- Ensure credential rotation is practiced and can be invoked during recovery.
- Map cross-account dependencies (trust policies, resource policies, KMS grants, integrations) and keep an inventory in the recovery runbook.
- Exercise the full workflow regularly (investigation → validation → rebuild → restore → cutover).

---

## Takeaways for an internship report

- Recovery must assume production identities and even backups can be targeted.
- Isolate recovery with separate accounts and an IRE to contain tainted restores.
- Treat validation as a pipeline (malware + integrity + audit), not a single check.
- Use Rebuild–Restore–Rotate to decide what to recover from code vs backup vs fresh issuance.

