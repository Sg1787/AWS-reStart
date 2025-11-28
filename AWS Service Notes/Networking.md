# 🌐 AWS Networking Fundamentals: Comprehensive Notes & Deep Dive

---

## 1. Foundational Concepts & IP Addressing Deep Dive

The foundation of AWS networking relies on standard **TCP/IP** principles and specific **CIDR** (Classless Inter-Domain Routing) configurations.

| Topic | Summary | Key Details |
| :--- | :--- | :--- |
| **IP Addressing** | **IPv4** addresses (32-bit) are finite. **IPv6** addresses (128-bit) offer a vast, next-generation address space. | **VPC IPs** are always in private ranges (e.g., $10.0.0.0/16$). |
| **IP Subnetting**| Logically dividing the main VPC CIDR block into smaller sub-networks. | A **/24** subnet (e.g., $10.0.1.0/24$) contains 256 addresses, but AWS **reserves 5 IPs** for internal use (network address, router, DNS, future use, broadcast address). |
| **Public vs. Private**| **Public IPs** (including **Elastic IPs**) are routable on the Internet. **Private IPs** are isolated within the VPC. | **Use Case:** Always use **Elastic IP (EIP)** for stable, long-term public addressing on instances that cannot tolerate IP changes. |
| **DHCP** | **Dynamic Host Configuration Protocol**. Automatically assigns private IP addresses to EC2 instances upon launch. | AWS runs its own **DHCP Option Set** within the VPC to manage IP assignment. |

---

## 2. Amazon VPC Core Components & Traffic Flow

The VPC is the heart of your cloud environment. Understanding its components and how traffic moves is crucial.

### VPC Architecture Components

* **VPC:** Your logical isolation boundary. Must be defined with a non-overlapping CIDR block (e.g., $10.0.0.0/16$).
* **Subnets:** **AZ-scoped** partitions of your VPC. Resources must reside in a Subnet.
* **Internet Gateway (IGW):** Enables communication for **Public Subnets**. It provides a target for the public Route Table's default route.
* **Route Tables:** Control the routing for **outbound traffic** originating from a subnet.
    * **Local Route:** The default route created automatically (e.g., $10.0.0.0/16$ -> `local`).
    * **Internet Route:** Routes $0.0.0.0/0$ to the IGW.
* **NAT Gateway:** Used in a **Public Subnet** to grant internet access to instances in **Private Subnets**. It is a **managed service** (highly available and requires an EIP).
    * **Use Case:** Allowing a database server in a Private Subnet to download OS patches, update certificates, or communicate with AWS services without being directly exposed to inbound internet traffic.

### The Lifecycle of an Inbound Request

1.  A request hits the **IGW**.
2.  The IGW checks the **Route Table** associated with the destination subnet.
3.  The Route Table directs the traffic based on the destination IP (usually to the `local` route or the IGW).
4.  The traffic is checked against the **Network ACL (NACL)** attached to the subnet.
5.  The traffic is checked against the **Security Group (SG)** attached to the EC2 instance.
6.  If all checks pass, the packet reaches the instance.

---

## 3. Network Security & Firewall Layers Best Practices

The combination of NACLs and Security Groups provides defense-in-depth network security.

### Security Group (SG) Rules: Stateful (Connection Tracking)

* **Scope:** Applied to **individual resources** (EC2, RDS, ALB, etc.).
* **Rule Type:** **Allow** only. If no rule explicitly allows traffic, it is denied.
* **Best Practice:** Always reference other **SGs** in rules (e.g., "Allow traffic from `sg-db-servers`") instead of referencing hardcoded private IPs.
* **Use Case Example:** Allowing only port 443 (HTTPS) from $0.0.0.0/0$ (the internet) for a web server, but only allowing port 3306 (MySQL) from the SG of the Web Tier.

### Network ACL (NACL) Rules: Stateless (No Connection Tracking)

* **Scope:** Applied to an entire **Subnet**.
* **Rule Type:** Supports both **Allow** and **Deny** rules, processed in **numbered order** (lowest number first, e.g., Rule 10 is processed before Rule 100).
* **Stateless Requirement:** If you allow inbound traffic on port 80, you must also explicitly allow **outbound traffic** on ephemeral ports (1024-65535) for the response to return.
* **Use Case Example:** Creating a **Deny** rule (e.g., Rule 50: Deny all traffic from a specific malicious IP range) that is evaluated before any general Allow rules.

---

## 4. Advanced Traffic & Global Connectivity Components

### Traffic Management (Load Balancers & Health Checks)

Load balancers distribute traffic to **Target Groups**, which contain the underlying compute resources (EC2 instances).

| Load Balancer Type | Layer | Target Group & Health Check Detail |
| :--- | :--- | :--- |
| **ALB (Application)** | L7 (HTTP/HTTPS) | Supports health checks based on an application **path** (e.g., `/healthcheck`). |
| **NLB (Network)** | L4 (TCP/UDP) | Health checks use TCP/HTTP/HTTPS and are designed for ultra-low latency; NLB uses **static IP addresses** per AZ. |
| **AWS Global Accelerator (A-GWA)** | L3/L4 | **Newer Service.** Uses the AWS global network backbone and **Edge Locations** to route user traffic to the closest healthy regional endpoint, drastically improving performance for global users. |

### Domain Name Service (Route 53)

* **Amazon Route 53:** AWS's **highly available and scalable DNS service**. It translates domain names (e.g., `example.com`) to IP addresses.
* **Use Case:** Using **Alias Records** to map your domain directly to AWS resources (like an ALB or S3 bucket) instead of an IP address, providing faster updates and cleaner integration.

### Secure Private Connectivity (VPC Endpoints)

* **VPC Endpoints (PrivateLink):** Enables you to privately connect your VPC to supported AWS services (e.g., S3, DynamoDB, SQS) **without requiring an Internet Gateway, NAT Gateway, or public IPs.**
* **Use Case:** Ensuring that data transferred to **S3** from your private EC2 instance never leaves the secure AWS network, which is critical for compliance and security.

### Hybrid and Inter-VPC Connections

* **AWS Transit Gateway (TGW):** The standard for complex network topologies. It acts as a **central router** for all VPCs and on-premises networks.
    * **Benefit:** Reduces the number of required connections from $N * (N-1) / 2$ (peering) to $N$ connections to the TGW.
* **AWS Direct Connect (DX):** A **private, dedicated** physical connection between your data center and AWS.
* **AWS Site-to-Site VPN:** Creates an **encrypted tunnel** over the **public internet** to connect your on-premises network to your VPC. Cheaper and faster to deploy than DX.

---

## 5. Security, Monitoring & Compliance

| Component | Description | Example Use Case |
| :--- | :--- | :--- |
| **VPC Flow Logs** | Captures **IP traffic metadata** (source, destination, port, action). | Diagnosing why a connection attempt is being **rejected** by an SG or NACL. |
| **AWS WAF (Web Application Firewall)** | Protects web applications from common web exploits (e.g., SQL injection, XSS) at **Layer 7**. | Blocking common malicious attack patterns targeting your public facing ALB. |
| **AWS Artifact** | Provides compliance reports (e.g., **SOC, PCI**) to prove that **AWS** meets global standards. | Presenting an auditor with AWS's own certification documents to meet compliance requirements. |
| **Amazon Inspector** | Automated security assessment that scans your **EC2 instances** for vulnerabilities. | Scanning a production web server before launch to identify vulnerable software versions. |
