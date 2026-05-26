
+++
title = "IAM Privilege Escalation Checker in Go"
description = "AWS IAM privilege escalation path analyzer and automated remediation generator, written in Go."
type = ["projects", "post"]
date = "2026-05-26"
[author]
name = "Francesco Citti"
+++

## Overview

A Go command line tool that reads an AWS IAM configuration and maps every path an attacker could use to reach administrator access. For each finding it generates a ready to deploy fix: a deny based IAM policy and Terraform configuration that blocks the escalation path without touching any other permissions.

```
go-escalation-check scan --snapshot testdata/sample_snapshot.json

Loading IAM snapshot: testdata/sample_snapshot.json
Loaded  4 users  3 roles  2 groups  4 managed policies

+-----------------+------+-------------------------------------+-----------+----------+
|    PRINCIPAL    | KIND |             TECHNIQUE               |   MITRE   | SEVERITY |
+-----------------+------+-------------------------------------+-----------+----------+
| alice           | user | Create New Policy Version           | T1484.001 | CRITICAL |
| alice           | user | Attach Admin Policy to User         | T1098.003 | CRITICAL |
| alice           | user | Pass Admin Role via EC2             | T1548     | HIGH     |
| carol           | user | Backdoor Role Trust Policy          | T1078.004 | CRITICAL |
| DeployRole      | role | Put Inline Admin Policy on Role     | T1098.003 | CRITICAL |

37 finding(s) across 6 principal(s)
```

## The Problem

In AWS, administrator access does not require already being an administrator. Given the right combination of IAM permissions, even seemingly harmless ones, an identity can quietly grant itself full access. This is privilege escalation.

* A user with `iam:AttachUserPolicy` can attach `AdministratorAccess` directly to their own account.
* A user with `iam:PassRole` and `ec2:RunInstances` can launch a server with an admin role attached and execute commands through it.
* A user with `iam:CreateRole` and `iam:AttachRolePolicy` can create a brand new admin role and assume it.

These combinations are hard to spot manually in accounts with many users, roles, groups, and layered policies. The tool automates the full analysis: 18 known escalation techniques checked against every identity in an account.

## What It Produces

| Output | Description |
|---|---|
| Table or JSON | Every finding: which identity, which technique, MITRE ATT&CK technique ID, severity |
| JIT policies | Deny based IAM policies scoped to only the dangerous actions per identity, with MFA requirement and 8 hour expiry condition |
| Terraform HCL | Ready to apply infrastructure code that deploys those policies to the account |

## Escalation Techniques Detected

| Technique | MITRE |
|---|---|
| Create New Policy Version | T1484.001 |
| Set Default Policy Version | T1484.001 |
| Attach Admin Policy to User / Group / Role | T1098.003 |
| Put Inline Admin Policy on User / Group / Role | T1098.003 |
| Create Access Key for Admin User | T1098.001 |
| Create Console Login for Admin User | T1098 |
| Reset Admin User Password | T1098 |
| Add Self to Admin Group | T1098 |
| Backdoor Role Trust Policy | T1078.004 |
| Pass Admin Role via EC2 / Lambda / CloudFormation / Glue | T1548 |
| Create and Assume New Admin Role | T1136.003 |

## Tech Stack

| Component | Detail |
|---|---|
| Language | Go 1.22 |
| IAM source | Live AWS API or offline JSON snapshot |
| Output formats | Table, JSON, JIT policies, Terraform HCL |
| Dependencies | cobra, aws-sdk-go-v2 |

## GitHub Repository

[github.com/FrancescoCitti/go-escalation-check](https://github.com/FrancescoCitti/go-escalation-check)
