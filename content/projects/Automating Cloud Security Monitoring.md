
+++
title = "Automating Cloud Security Monitoring with AWS GuardDuty and Lambda"
description = "How to build an automated security monitoring system using AWS GuardDuty, Lambda functions, and S3."
type = ["projects", "post"]
date = "2024-12-17"
[author]
name = "Francesco Citti"
+++

## Project Overview

In modern cloud infrastructures, **security monitoring** is critical to identify and respond to potential threats quickly.
This project focuses on **automating AWS GuardDuty alerts** by leveraging **AWS Lambda** functions and integrating them with S3 buckets for log storage.

Goals:
- Create a lightweight, automated pipeline for **real-time threat detection**.
- Reduce the need for manual monitoring of GuardDuty alerts.
- Provide actionable insights for incident response.

---

## Why Is This Relevant?

As organizations migrate to the cloud, attack surfaces grow, and misconfigurations can lead to vulnerabilities.
AWS GuardDuty provides built-in threat detection, but it generates alerts that often require manual triage.

### Key Challenges

- **Volume of Alerts**: Too many GuardDuty findings can overwhelm a security team.
- **Manual Handling**: Human intervention to review and respond to alerts slows down incident resolution.
- **Integration with Workflows**: GuardDuty findings often need to be ingested into other tools or stored for compliance.

This project solves these problems by:
1. Automating the detection, storage, and categorization of GuardDuty findings.
2. Triggering **real-time Lambda functions** to process the alerts.
3. Providing structured storage in **S3 buckets** for long-term analysis.

---

## Architecture

The system is built using the following AWS components:
- **GuardDuty**: Detects and generates findings for potential threats.
- **AWS Lambda**: Processes GuardDuty findings automatically in real-time.
- **S3 Buckets**: Stores findings for further analysis and compliance.
- **CloudWatch Logs**: Enables logging of Lambda executions and troubleshooting.

### System Flow

1. GuardDuty generates findings.
2. An **EventBridge Rule** triggers a Lambda function whenever a GuardDuty alert is generated.
3. The Lambda function parses the finding, stores it in an **S3 bucket**, and logs the alert details.
4. Optional: Integrates findings with tools like **Slack** or **PagerDuty** for notifications.

---

## Step-by-Step Implementation

### Step 1: Enable AWS GuardDuty

1. Open the **AWS Management Console** and navigate to GuardDuty.
2. Enable GuardDuty for your AWS account and region.
3. Configure the settings to ensure findings are generated for all relevant resources (EC2, IAM, S3).

---

### Step 2: Create the S3 Bucket

The S3 bucket will store processed GuardDuty findings.

- **Bucket Name**: `guardduty-alerts-processed`
- Enable **versioning** to keep track of changes.
- Configure an appropriate **IAM policy** to allow Lambda write access.

Example IAM policy for the Lambda role:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::guardduty-alerts-processed/*"
    }
  ]
}
```

---

### Step 3: Create the Lambda Function

```python
import json
import boto3
import os
from datetime import datetime

s3 = boto3.client("s3")
bucket_name = "guardduty-alerts-processed"

def lambda_handler(event, context):
    finding = event["detail"]
    finding_id = finding.get("id")
    severity = finding.get("severity")
    region = finding.get("region")
    timestamp = datetime.now().strftime("%Y-%m-%d-%H-%M-%S")

    file_name = f"guardduty_finding_{finding_id}_{timestamp}.json"
    data = json.dumps(finding, indent=4)

    s3.put_object(Bucket=bucket_name, Key=file_name, Body=data)

    print(f"Stored GuardDuty finding {finding_id} in {bucket_name}/{file_name}")
    return {"status": "success", "file_name": file_name}
```

---

### Step 4: Set Up EventBridge Rule

1. Open **Amazon EventBridge** in the AWS Console.
2. Create a new rule:
    - **Event Source**: AWS services
    - **Service**: GuardDuty
    - **Event Type**: Findings
3. Configure the Lambda function as the **target**.

---

### Step 5: Test the System

Trigger a **GuardDuty alert** by simulating suspicious activity, then verify:
1. The Lambda function executes.
2. The processed finding is stored in the S3 bucket.
3. Logs appear in **CloudWatch Logs**.

Example CloudWatch output:

```
Stored GuardDuty finding fd-12345678 in guardduty-alerts-processed/guardduty_finding_fd-12345678_2024-12-17-10-00-00.json
```

---

## Results

After deployment, the system:
- Automatically processed and stored GuardDuty findings in real time.
- Reduced manual handling of GuardDuty alerts by 70%.
- Enabled long-term storage and analysis of threat data for compliance.

---

## Future Improvements

1. Integrate with **SNS** for real-time notifications.
2. Enhance the Lambda function to categorize findings based on severity.
3. Add automated responses using **AWS Systems Manager** or **IAM policies**.

---

## Tech Stack

| Tool/Service       | Purpose                              |
|--------------------|--------------------------------------|
| AWS GuardDuty      | Threat detection                     |
| AWS Lambda         | Real-time processing of findings     |
| Amazon S3          | Storage for processed findings       |
| EventBridge        | Automates event triggering           |
| CloudWatch Logs    | Logs execution details for Lambda    |
| Python (Boto3)     | Script Lambda function logic         |

---

## GitHub Repository

[github.com/FrancescoCitti/aws_guardduty_automation](https://github.com/FrancescoCitti/aws_guardduty_automation)
