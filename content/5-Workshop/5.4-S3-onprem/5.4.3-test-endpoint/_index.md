---
title : "Configure RDS Multi-AZ, Secrets Manager, and RDS Proxy"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

## Objectives

Set up a highly available and secure data layer so the backend can read/write data without directly accessing the database endpoint.

## Implementation steps

1. **Create DB Subnet Group**
   - Select private subnets dedicated for data layer.
   - Ensure subnet group is evenly distributed across 2 Availability Zones.

2. **Create Amazon RDS MySQL Multi-AZ**
   - Select appropriate engine, instance class, storage, and Multi-AZ mode.
   - Configure backup retention, maintenance window, and security group.

3. **Store secrets in Secrets Manager**
   - Store username, password, and database connection variables.
   - Avoid hardcoding login information in the application.

4. **Create AWS RDS Proxy**
   - Attach proxy to the just-created database.
   - Configure authentication using secret in Secrets Manager.
   - Specify security group and subnet for proxy.

5. **Integrate backend with RDS Proxy**
   - Update backend connection string to point to proxy endpoint.
   - Test data read/write operations and confirm application still works correctly.

## Expected results

After this step, backend connects to the data layer through RDS Proxy, reducing connection exhaustion risk and increasing transparent failover capability.

## Suggested images to add

- DB Subnet Group.
- RDS MySQL Multi-AZ.
- Secret in Secrets Manager.
- RDS Proxy endpoint and connection test from backend.
