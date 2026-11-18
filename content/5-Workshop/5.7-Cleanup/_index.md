---
title: "Clean up resources"
date: 2026-11-18
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

This section records cleanup of the existing Tindy environment through CloudFormation in **N. Virginia (`us-east-1`)**. Cleanup precedes redeployment of six stacks, a **Fresh** database, bootstrap, migrations and demo seeding. The web demonstration will use a new CloudFront hostname.

## Verified progress

| Component | Recorded state |
| --- | --- |
| Edge | Maintenance verified, termination protection disabled and stack deletion confirmed as `DELETE_COMPLETE`. |
| Runtime | ALB ingress switched to the CloudFront prefix list. Stack deletion completed. |
| Database | Deletion protection disabled and stack deletion verified as `DELETE_COMPLETE`. |
| Integrations | Stack and displayed resources report `DELETE_COMPLETE`. |
| Network | Termination protection disabled and deletion verified as `DELETE_COMPLETE`. |
| Foundation | Mapping exported and stack deletion verified as `DELETE_COMPLETE`. Retained resources report `DELETE_SKIPPED`. |

## Cleanup steps

| Guide page |
| --- |
| [5.7.1 Review infrastructure before cleanup](5.7.1-Inventory/) |
| [5.7.2 Enable CloudFront maintenance](5.7.2-Maintenance/) |
| [5.7.3 Prepare and delete Runtime](5.7.3-Runtime/) |
| [5.7.4 Disable protection and delete Database](5.7.4-Database/) |
| [5.7.5 Review and delete Integrations](5.7.5-Integrations/) |
| [5.7.6 Disable protection and delete Edge](5.7.6-Edge/) |
| [5.7.7 Disable protection and delete Network](5.7.7-Network/) |
| [5.7.8 Export mappings and delete Foundation](5.7.8-Foundation/) |
| [5.7.9 Cleanup results and redeployment preparation](5.7.9-Results/) |
