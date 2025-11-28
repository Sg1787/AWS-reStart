# ☁️ AWS Architecture and Frameworks

Notes on core AWS guiding principles, architectural frameworks, and migration strategies.

---

## 1. AWS Architecture Overview

The fundamental AWS architecture uses global infrastructure organized into **Regions** (geographic areas) and **Availability Zones (AZs)** (isolated data centers within a Region). Applications are typically built using modular components across three layers:

| Component/Concept | Summary/Use |
| :--- | :--- |
| **Region** | A physical location with multiple isolated **Availability Zones**. Select a Region for latency, cost, and compliance needs. |
| **Availability Zone (AZ)** | One or more discrete data centers with redundant power, networking, and connectivity. Used to achieve **High Availability (HA)**. |
| **Amazon VPC** | A logically isolated virtual network where you launch AWS resources. Acts as your private data center in the cloud. |
| **EC2** | **Elastic Compute Cloud**—provides resizable compute capacity (virtual servers) in the cloud. |
| **S3** | **Simple Storage Service**—object storage built for **99.999999999% (11 nines)** durability. Used for data lakes, backups, and static website hosting. |
| **ELB** | **Elastic Load Balancing**—automatically distributes incoming application traffic across multiple targets (e.g., EC2 instances) to ensure performance and reliability. |
| **IAM** | **Identity and Access Management**—securely controls access to AWS services and resources for users and services using policies, users, groups, and roles. |

---

## 2. AWS Cloud Adoption Framework (AWS CAF)

AWS CAF provides guidance and best practices to help organizations digitally transform and accelerate business outcomes through the cloud. It groups capabilities into six perspectives:

| Perspective | Focus/Stakeholders |
| :--- | :--- |
| **Business** | Ensuring cloud investments align with and accelerate **business outcomes** and strategy. (CEO, CFO, COO) |
| **People** | Bridging the gap between business and technology; focusing on **organizational culture**, structure, and workforce transformation. |
| **Governance** | Maximizing organizational benefits and minimizing risks through **portfolio management**, program management, and financial control. |
| **Platform** | Designing and building the cloud environment, focusing on **architecture** and data engineering. |
| **Security** | Achieving **confidentiality, integrity, and availability** of data and cloud workloads. |
| **Operations** | Defining processes for managing cloud services, including **observability**, incident management, and release management. |

---

## 3. AWS Well-Architected Framework

A set of best practices and design principles for designing and operating reliable, secure, efficient, and cost-effective systems in the cloud. It is structured around six core pillars. 

### Well-Architected Principles

| Pillar | Focus/Goal | Core Design Principle |
| :--- | :--- | :--- |
| **1. Operational Excellence** | Run and monitor systems to deliver business value and continually improve supporting processes. | **Perform operations as code** (using Infrastructure as Code, e.g., AWS CloudFormation). |
| **2. Security** | Protecting information, systems, and assets. | **Implement a strong identity foundation** (using IAM, MFA). **Automate security best practices**. |
| **3. Reliability** | The ability of a system to recover from infrastructure or service failures and dynamically acquire computing resources to meet demand. | **Automatically recover from failure**. **Test recovery procedures**. |
| **4. Performance Efficiency** | Using computing resources efficiently to meet system requirements and maintain that efficiency as demand changes. | **Democratize advanced technologies** (using managed services). **Scale horizontally** instead of vertically. |
| **5. Cost Optimization** | Avoiding unnecessary costs and achieving the lowest possible price. | **Adopt a consumption model** (pay-as-you-go). **Measure overall efficiency** (use AWS Cost Explorer). |
| **6. Sustainability** | Minimizing the environmental impacts of running cloud workloads. | **Maximize utilization** and adopt efficient data storage and networking patterns. |

---

## 4. Reliability and High Availability

* **Reliability:** The ability of a system to perform its intended function correctly and consistently when required.
* **High Availability (HA):** Ensuring the workload is available to users even when a component fails. Achieved by deploying across **multiple Availability Zones**.

| Code/Service | Summary/Use for HA/Reliability |
| :--- | :--- |
| **Multi-AZ** | Deploying resources (e.g., RDS) to multiple AZs for automatic failover and data redundancy. |
| **Auto Scaling** | Automatically adjusts the number of EC2 instances to meet demand and replaces unhealthy instances. |
| **Route 53 Health Checks** | Used with DNS records to redirect traffic away from failed endpoints or Regions. |

---

## 5. Transitioning a Data Center to the Cloud

The migration process often follows the AWS Migration Acceleration Program (MAP) structure, leveraging the **6 R's** migration strategies:

### 6 R's of Migration Strategy

| Strategy | Summary |
| :--- | :--- |
| **Rehost (Lift and Shift)** | Moving an application as-is to the cloud (e.g., VM to **EC2**). Quickest, but minimal cloud optimization. |
| **Replatform (Lift, Tinker, and Shift)** | Moving to the cloud and making a few minor, cloud-optimizing changes (e.g., moving from a self-managed database on EC2 to **Amazon RDS**). |
| **Refactor/Re-Architect** | Rebuilding the application using cloud-native features (e.g., monolithic app to **microservices** using **Lambda** and **DynamoDB**). Highest cost and time, but maximum cloud benefits. |
| **Repurchase** | Moving to a different product, typically a SaaS solution (e.g., moving from on-prem CRM to Salesforce). |
| **Retire** | Decommissioning applications that are no longer needed. |
| **Retain** | Keeping some applications in the source environment (on-premises) due to regulatory constraints or pending modernization efforts. |
