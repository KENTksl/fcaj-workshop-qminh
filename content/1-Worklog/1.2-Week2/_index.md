---
title: "Week 2 Worklog"
date: 2026-04-27
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
### Week 2 Objectives:

* Complete Module 2 and strengthen core AWS networking knowledge (VPC, subnets, routing, and security controls).
* Understand network security and observability mechanisms in VPC (Security Group, NACL, VPC Flow Logs).
* Understand connectivity patterns for AWS and hybrid environments (VPN, Direct Connect) and distinguish ELB load balancer types.
* Complete the planned labs: Lab 3 (VPC), Lab 10 (Route 53), Lab 19 (VPC Peering), Lab 20 (Transit Gateway), Lab 58 (System Manager - Session Manager).

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Review and organize Module 2 based on an AWS network design flow <br>&emsp; + VPC overview and network isolation <br>&emsp; + Public/private subnet design and AZ considerations <br>&emsp; + Route tables and routing behavior <br>&emsp; + Security Group (stateful) and inbound/outbound control <br>&emsp; + Internet Gateway for public subnets <br>&emsp; + NAT Gateway + Elastic IP for outbound Internet access from private subnets                                                                                             | 27/04/2026 | 27/04/2026      | https://youtu.be/O9Ac_vGHquM?si=fym2d8H5rZwqmaPe <br> https://000003.awsstudygroup.com |
| 3   | - Extended study of Module 2: security, observability, and connectivity patterns in VPC <br>&emsp; + VPC firewall layers: Security Group (stateful, ENI-level, allow-only) vs Network ACL/NACL (stateless, subnet-level, allow/deny) <br>&emsp; + Network monitoring: VPC Flow Logs (accepted/rejected traffic for troubleshooting) <br>&emsp; + VPC connectivity: VPC Peering (no transitive routing, no CIDR overlap) vs Transit Gateway (hub-and-spoke for multi-VPC/on-prem) <br>&emsp; + Elastic Network Interface (ENI) use cases <br>&emsp; + VPC Endpoint for private access to AWS services without relying on Internet/NAT                                            | 28/04/2026 | 28/04/2026      | https://youtu.be/BPuD1l2hEQ4?si=e8W3LjDBQowBDSHW <br> https://000003.awsstudygroup.com |
| 4   | - Study hybrid networking models and Elastic Load Balancing within AWS network architecture <br>&emsp; + Hybrid connectivity: VPN Site-to-Site, VPN Client-to-Site, AWS Direct Connect (high bandwidth/stability/low latency) <br>&emsp; + Elastic Load Balancing (ELB): distributing requests across multiple targets <br>&emsp; + Load balancer types: ALB (L7), NLB (L4), Classic LB (legacy), Gateway LB (security) <br>&emsp; + Operational mechanisms: health checks and sticky sessions                                                                                         | 29/04/2026 | 29/04/2026      | https://youtu.be/CXU8D3kyxIc?si=tDkD9doFXV6UH8Pc <br> https://000003.awsstudygroup.com |
| 5   | - **Lab 3 (Hands-on):** Build a complete VPC to simulate an environment with public/private subnets <br>&emsp; + Create a VPC and subnets (public/private) <br>&emsp; + Create and associate route tables <br>&emsp; + Create security groups for inbound/outbound control <br>&emsp; + Create an Internet Gateway and configure routes for public subnets <br>&emsp; + Allocate an Elastic IP and create a NAT Gateway for outbound access from private subnets                                            | 30/04/2026 | 30/04/2026      | https://000003.awsstudygroup.com |
| 6   | - **Hands-on labs:** <br>&emsp; + Lab 10: Route 53 <br>&emsp; + Lab 19: VPC Peering <br>&emsp; + Lab 20: Transit Gateway <br>&emsp; + **Lab58:** Working with Amazon System Manager - Session Manager (create connections to public/private instances in VPC) <br>&emsp; + Week 2 recap                                                                                         | 01/05/2026 | 01/05/2026      | https://000010.awsstudygroup.com/ <br> https://000019.awsstudygroup.com/ <br> https://000020.awsstudygroup.com/<br> https://000058.awsstudygroup.com/ |


### Week 2 Achievements:

* Organized Module 2 knowledge using an AWS networking design perspective:
  * VPC for network isolation
  * Public/private subnet design across AZs
  * Route tables and traffic routing behavior
  * Internet Gateway and NAT Gateway (with Elastic IP) for Internet access patterns
* Understood key security layers and their scopes:
  * Security Group: stateful, ENI-level, allow-only rules
  * Network ACL (NACL): stateless, subnet-level, allow/deny rules
* Learned to use VPC Flow Logs for network troubleshooting by analyzing accepted/rejected traffic.
* Understood connectivity models:
  * VPC Peering: direct VPC-to-VPC connectivity, no transitive routing, not compatible with overlapping CIDR ranges
  * Transit Gateway: hub-and-spoke connectivity for multiple VPCs and/or on-premises networks
  * Hybrid connectivity: VPN Site-to-Site, VPN Client-to-Site, and AWS Direct Connect
* Understood the purpose of Elastic Load Balancing (ELB) and the differences between load balancer types:
  * ALB (Layer 7), NLB (Layer 4), Classic LB (legacy), Gateway LB (security)
  * Operational concepts such as health checks and sticky sessions
* Completed the planned labs:
  * Lab 3: built a VPC with public/private subnets and required routing/security components
  * Lab 10: Route 53
  * Lab 19: VPC Peering
  * Lab 20: Transit Gateway
  * Lab 58: Working with Amazon System Manager - Session Manager
