---
title: "RDS Automated Backup"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.9.1. </b> "
---

## Objectives

Set up RDS Automated Backup to automatically create daily snapshots, retaining them for 7 days.

## Implementation steps

1. Go to **RDS → Databases → globalmart-db → Modify**.
2. Scroll down to **Additional configuration**:
   - **Backup retention period**: Select `7 days`.
   - **Backup window**: Select `18:00 - 19:00 UTC` (equivalent to 01:00-02:00 AM Vietnam time) → choose off-peak hours to avoid affecting performance.
3. Click **Continue → Apply immediately → Modify DB instance**.
4. Wait a few seconds, RDS Status returns to **Available**.
![Overview](/images/5-Workshop/Workshop-img/rds_auto-1.jpg)

### Verify backup is enabled

1. Go to **RDS → Databases → globalmart-db → Maintenance & backups tab**.
2. Check the "Automated backups" section → You should see: `Backup retention: 7 days`.

### View existing snapshots

1. Go to **RDS → Automated backups → select globalmart-db**.
2. See the list of snapshots by day.

## Expected results

Every day at 01:00 AM Vietnam time, RDS automatically creates 1 snapshot. Retains the last 7 days, automatically deletes older ones.
