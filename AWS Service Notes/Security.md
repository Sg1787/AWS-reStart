# 🛡️ AWS Security Notes

This document provides a simple, straight-to-the-point overview of essential AWS security concepts and services, organized by the security life cycle.

---

## 🔑 Codes Used and Their Uses

| Code/Service | Use | Area |
| :--- | :--- | :--- |
| **IAM** | Manages access to AWS services and resources securely (users, groups, roles, policies). | Identity Management |
| **CloudTrail** | Logs all AWS API activity/actions for auditing and monitoring. | Detection & Monitoring |
| **AWS Config** | Continuously monitors and assesses the configurations of AWS resources for compliance. | Detection & Compliance |
| **Trusted Advisor** | Provides real-time guidance on optimizing performance, fault tolerance, cost, and **security**. | Analysis & Best Practices |
| **ACM** | Provision, manage, and deploy SSL/TLS certificates for encryption. | Data Security/PKI |
| **KMS** | Manages the encryption keys used for data at rest. | Data Security |
| **WAF** | Protects web applications from common web exploits (e.g., SQL injection, cross-site scripting). | Network Hardening |
| **Amazon Inspector** | Automated security assessment service for EC2 instances and container images. | Detection & Analysis |

---

## 1. Introduction to Security

### Introduction to Security
* **Shared Responsibility Model:** AWS secures the **"Security *of* the Cloud"** (physical infrastructure, global network). The customer is responsible for the **"Security *in* the Cloud"** (data, IAM, network settings, OS patching).

### Acceptable Use Policy Example
* **Purpose:** Prohibits illegal, harmful, or offensive uses of AWS services, such as distributing malware or conducting unauthorized penetration testing against AWS infrastructure.

---

## 2. Prevention (Security Life Cycle)

Prevention focuses on implementing controls to stop threats before they happen.

### Prevention: Identity Management
* **Identity Management:** The process of ensuring the right users/services have the correct level of access to resources.
* **AWS Identity and Access Management (IAM):** A global service used to create and manage AWS users, groups, and roles, and define their permissions using **policies** (Principle of Least Privilege).

### Prevention: Public Key Infrastructure (PKI)
* **Public Key Infrastructure:** A system used to create, manage, and revoke **digital certificates** to enable secure communication via encryption.
* **Amazon Certificate Manager (ACM):** Simplifies PKI by provisioning, managing, and automatically renewing SSL/TLS certificates for use with AWS services (e.g., Load Balancers, CloudFront).
    * **ACM Demonstration:** Shows how to request and deploy a free SSL/TLS certificate managed by AWS.

### Network-Hardening
* **Goal:** Restrict unwanted network traffic and protect the perimeter.
* **Tools:** Using **VPC**, **Security Groups** (instance-level firewall), **NACLs** (subnet-level firewall), and **AWS WAF** (application layer protection).

### Prevention: System Hardening
* **Goal:** Secure the operating system and applications on compute resources (like EC2).
* **Actions:** Regularly patch the OS, disable unnecessary services/ports, and use secure, pre-configured **Amazon Machine Images (AMIs)**.

### Prevention: Data Security / Data Protection
* **Data Protection:** Protecting data both **at rest** (stored) and **in transit** (moving).
* **Data Security:** Use **AWS KMS** and **S3 encryption** for data at rest. Use **TLS/SSL** (enabled via ACM) for data in transit. Enforce access controls using IAM.

---

## 3. Detection and Response

Detection focuses on identifying security events and misconfigurations.

### Detection
* **AWS CloudTrail:** The primary detection tool; logs every API call/action made in the account, providing an audit trail.
* **AWS Config:** Detects and flags when a resource's configuration deviates from the defined policy (e.g., an S3 bucket is made public).
* **Firewall Malware:** Services like **Amazon GuardDuty** provide intelligent threat detection to monitor for malicious activity (e.g., unauthorized access, compromised EC2 instances).
* **External Tool:** Integration with third-party **SIEM** (Security Information and Event Management) tools to aggregate and analyze logs.

### Response
* **Action:** The steps taken immediately after an event is detected. This typically involves isolating the compromised resource and revoking associated credentials.

### Analysis
* **Function:** Post-incident review to understand the root cause and scope of a security event using tools like CloudTrail logs and VPC Flow Logs.

### Monitor an EC2 Instance
* **Monitoring:** Use **Amazon CloudWatch** for performance metrics and logs. Use **Amazon Inspector** to continuously scan the instance for security vulnerabilities.

---

## 4. Compliance and Best Practices

### AWS Trusted Advisor
* **Function:** Scans your environment and provides automated recommendations for **Security**, Cost Optimization, Performance, Fault Tolerance, and Service Limits.
    * **Security:** Checks for best practices like MFA on the root account and open ports.

### Security Best Practices
* Enable **Multi-Factor Authentication (MFA)** on the root account.
* Never use the root user for daily tasks.
* Enforce the **Principle of Least Privilege** with IAM policies.
* Encrypt all sensitive data at rest and in transit.

### AWS Compliance Program
* **Program:** AWS maintains compliance with global standards (e.g., SOC, PCI, HIPAA) for the infrastructure they manage.
* **Tool:** **AWS Artifact** provides customers with on-demand access to AWS's compliance reports to help them with their own compliance obligations.

### AWS Security Resources
* **Resources:** Utilize the **AWS Well-Architected Framework's Security Pillar**, **AWS Security Hub**, and specific services like **GuardDuty** and **Macie** for comprehensive security management.

---
