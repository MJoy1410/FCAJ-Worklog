---
title: "Review infrastructure before cleanup"
date: 2026-11-18
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

## Record the existing stacks

1. Open **CloudFormation → Stacks** in `us-east-1`, select **Active**, and filter for `tindy`.
2. Confirm all six stacks exist. Foundation, Integrations, Runtime and Edge initially report `UPDATE_COMPLETE`. Network and Database report `CREATE_COMPLETE`. Finish active updates before proceeding.

**📸 Capture the six stack names and status.**

![Initial Tindy stacks](/images/5-Workshop/cleanup/cleanup-01-stacks-before.png)

3. Open **tindy-edge → Outputs**, record `DistributionId`, `DistributionDomainName` and `EdgeMode`, initially `live`.

![Initial Edge outputs](/images/5-Workshop/cleanup/cleanup-02-edge-before.png)
