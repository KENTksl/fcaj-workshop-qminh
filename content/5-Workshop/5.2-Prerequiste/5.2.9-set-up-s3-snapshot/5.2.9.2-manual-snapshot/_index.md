---
title: "Manual Snapshot"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.9.2. </b> "
---

## Objectives

Create a manual snapshot as a baseline before starting system operation.

## Implementation steps

1. Go to **RDS → Databases → globalmart-db**.
2. Click **Actions → Take snapshot**.
3. Enter **Snapshot name**: `globalmart-db-baseline`
4. Click **Take snapshot**.
5. Wait for Status to change to **Available**.
![Overview](/images/5-Workshop/Workshop-img/rds_snapshot.jpg)
## Expected results

You have a manual snapshot as the first recovery point.
