# ⚙️ AWS Systems Operations and Management

Key concepts, best practices, and tools for managing and troubleshooting systems on the AWS Cloud, including the AWS CLI and IAM best practices.

---

## 1. Systems Operations Overview

**Systems Operations (SysOps)** focuses on the effective, secure, and reliable deployment, maintenance, and monitoring of systems in the cloud. It encompasses tasks like monitoring, automation, security management, and incident response.

### Systems Operations on AWS

AWS provides various services to automate and simplify SysOps tasks:

* **Amazon CloudWatch:** Used for **monitoring** resources, applications, and logs.
* **AWS Systems Manager (SSM):** Provides a unified interface to view operational data and automate operational tasks across your AWS resources.
* **AWS CloudFormation:** Used for **Infrastructure as Code (IaC)** to provision and manage resources predictably.
* **AWS Config:** Used to assess, audit, and evaluate the configurations of your AWS resources.

---

## 2. Troubleshooting and Knowledge Management

### Create a Troubleshooting Knowledge Base

A **Knowledge Base (KB)** is a centralized repository of information, guides, and solutions used to resolve common incidents efficiently. It helps reduce mean time to resolution (**MTTR**) and ensures consistency across operations teams.

#### Knowledge Base Spreadsheet

A useful KB often starts as a spreadsheet or table tracking key information for recurring issues.

| Column | Purpose | Example |
| :--- | :--- | :--- |
| **Issue/Symptom** | What the user/system is reporting. | EC2 instance stuck in 'Initializing' status. |
| **Service/Component** | The affected AWS service or application component. | Amazon EC2 |
| **Resolution Steps** | The step-by-step instructions to fix the issue. | 1. Check **System Status Checks**. 2. Check associated **Security Group** rules. 3. Review **CloudWatch Logs**. |
| **Root Cause** | The underlying reason for the issue. | Security Group was incorrectly blocking the health check port. |
| **Preventative Action** | What was done to ensure the issue does not happen again. | Updated CloudFormation template to include correct Security Group rules. |

---

## 3. AWS Identity and Access Management (IAM) Review

**IAM** is fundamental to secure operations. It controls who is **authenticated** (can sign in) and **authorized** (what they can do) to use AWS resources.

| Concept | Summary/Use |
| :--- | :--- |
| **Principle of Least Privilege** | Granting only the permissions required to perform a task. **The fundamental security best practice.** |
| **IAM User** | Represents a person or service that interacts with AWS. Should use **MFA**. |
| **IAM Group** | A collection of IAM users. Permissions are applied to the group, simplifying management. |
| **IAM Role** | An identity that you can assume to get temporary permissions. Used by **AWS services** (e.g., EC2) or users/services to access resources in another account. |
| **IAM Policy** | A document (written in **JSON**) that defines permissions. Can be attached to Users, Groups, or Roles. |
| **MFA (Multi-Factor Authentication)** | Required for the **Root User** and strongly recommended for all IAM users accessing the console. |

---

## 4. AWS Command Line Interface (CLI)

The **AWS CLI** is a unified tool to manage your AWS services from the command line, automating tasks through scripts.

### Introduction and Install and Practice Using the AWS CLI

1.  **Installation:** Install the appropriate package for your OS (Windows, macOS, Linux).
2.  **Configuration:** Use `aws configure` to set up your **Access Key ID**, **Secret Access Key**, default **Region**, and output format.
3.  **Practice:** Start with simple commands to list resources.

#### Key AWS CLI Codes and Uses

| Code | Summary/Use |
| :--- | :--- |
| `aws configure` | Sets up the initial configuration (credentials and region). |
| `aws ec2 describe-instances` | **Lists** all EC2 instances and their details in the configured region. |
| `aws s3 ls` | **Lists** all S3 buckets owned by the account. |
| `aws s3 cp my-file.txt s3://my-bucket/` | **Copies** a local file to an S3 bucket. |
| `aws iam list-users` | **Lists** all IAM users in the account. |
| `aws autoscaling set-desired-capacity --auto-scaling-group-name ASG-Web --desired-capacity 4` | **Automates** an operational change (scaling up an Auto Scaling Group). |

### KC - System Operations

The core knowledge check (KC) for system operations revolves around **automation**, **monitoring**, **security/access management**, and **disaster recovery/high availability** using AWS native tools like CloudWatch, Systems Manager, IAM, and Multi-AZ deployments.
