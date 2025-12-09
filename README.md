# AWS re/Start Journey  
*“Rot is inevitable—but with proper IAM policies, even decay can be contained.”*  
— Nurgle, if he audited AWS accounts  

Hi, I’m **Sadiyah Grobbler**  
A curious builder, future **cloud security engineer**, and proud participant in the **AWS re/Start** program.

This repo is my living log of cloud learning—where I document labs, projects, and hard-won lessons in **AWS architecture, automation, and defense**. Everything here is a stepping stone toward securing digital realms from misconfigurations, overprivileged roles, and unencrypted secrets.

---

## About Me

- **Name:** Sadiyah Grobbler  
- **Background:** Transitioning into tech with a passion for **automation, serverless systems, and security**.  
- **Career Goal:** Become a **cloud security engineer**—designing systems that are resilient, compliant, and *secure by default*.  

---

##  AWS re/Start Journey

### Why I Joined
I wanted more than just cloud knowledge—I wanted **hands-on experience** with real-world scenarios, guided training, and a path into the tech industry. AWS re/Start offered structure, labs, mentorship, and a community. Plus… free labs. (Yay!)

### Key Lessons & Experiences
- Learned that **security starts at design**, not as an afterthought.
- Discovered the power of **infrastructure-as-code** (even if I’m still writing bash scripts like ancient runes).
- Realized that **“it works” ≠ “it’s secure”**—a truth that changed everything.

---

## Skills & Technologies

### Core AWS Services
- **Compute:** EC2 (virtual servers), Lambda (serverless functions)  
- **Storage:** S3 (object storage), EBS (block storage for EC2)  
- **Databases:** RDS (managed relational DBs), DynamoDB (NoSQL)  
- **Networking:** VPC (isolated networks), Route 53 (DNS), Security Groups  
- **Security:** IAM (identity & access), KMS (encryption), CloudWatch (logging)  

### Tools & Platforms
- **OS:** Linux (Ubuntu & Amazon Linux)  
- **Scripting:** Python (for automation & Lambda)  
- **Key Concepts:** Shared Responsibility Model, Least Privilege, Defense in Depth  

>  *"Give a user full access, and they’ll break the system. Give them less access, and they’ll learn to ask nicely."*

---

## Things Completed During the Program

###  Certifications & Badges (SimuLearn)
All completed on [AWS Skill Builder](https://skillbuilder.aws):

| Module | What I Learned | Relevance to Security |
|-------|----------------|------------------------|
| **Core Security Concepts** | Enforced least-privilege access for support engineers at a stock exchange. | IAM roles, policy scoping, minimizing attack surface. |
| **Networking Concepts** | Built a secure VPC architecture for a bank to control internet ↔ internal traffic. | Network segmentation, public/private subnets, security groups. |
| **File Systems in the Cloud** | Deployed shared, encrypted file storage for a pet modeling agency using EFS. | Secure data sharing, encryption at rest, managed services. |
| **Databases in Practice** | Migrated legacy DBs to RDS—automating patching & improving uptime. | Managed DB security, reduced human error, high availability. |


>  *"Data left unencrypted is data waiting to be stolen. Encrypt always—even your grocery lists."*

---

##  Projects

### 1. **Static Website on S3** *(Deadline: 28 Nov)*  
- Hosted a personal portfolio using **S3 + Route 53**.  
- Enforced **HTTPS**, **blocked public puts**, and enabled **logging**.  
- *Why?* Even simple sites need security hygiene.

### 2. **3D Cloud Architecture Diagram** *(Deadline: 5 Dec)*  
- Visualized a secure, multi-tier AWS architecture (web, app, DB layers).  
- Included **VPC isolation**, **IAM roles**, and **encryption flows**.  
- *Why?* Good security starts with clear design.

### 3. **AWS Lex Chatbot** *(TBD)*  
- TBA

---

##  AWS Core Services — Explained Simply

### Compute  
> “Where your code lives.”  
- **EC2**: Virtual servers you manage. Great for control, but **you patch them**.  
- **Lambda**: Run code without servers. AWS handles the OS—**less surface to secure**.  
- *Security Tip:* Always run with **least-privilege IAM roles**, never root!

### Networking  
> “How your resources talk—safely.”  
- **VPC**: Your private cloud inside AWS. No strangers allowed.  
- **Route 53**: Directs traffic (like a cloud GPS).  
- *Security Tip:* Keep databases in **private subnets**—no direct internet access!

### Storage  
> “Where your data rests.”  
- **S3**: For files (images, logs, backups). Use **bucket policies + encryption**.  
- **EBS**: Disk storage attached to EC2. Encrypt it—always.  
- *Security Tip:* **Block public access** by default. Assume everything is sensitive.

### Databases  
> “The heart of your app—protect it.”  
- **RDS**: Managed relational DBs (MySQL, PostgreSQL). AWS handles patching.  
- **DynamoDB**: Fast, serverless NoSQL. Scale without ops.  
- *Security Tip:* Never store credentials in code. Use **IAM auth or Secrets Manager**.

### Security  
> “The glue that holds it all together.”  
- **IAM**: Who can do what. **Principle of least privilege is law.**  
- **KMS**: Encrypt data at rest and in transit.  
- **CloudWatch + GuardDuty**: Watch for weirdness. Detect. Respond.  
- *Nurgle’s Law:* “If it’s not logged, it didn’t happen. If it’s not encrypted, it’s already stolen.”

---

## Contact Me

- **Email:** sgrobbler1@gmail.com
- **Cell:** +27 793408114
- **LinkedIn:** [Sadiyah Grobbler](https://www.linkedin.com/in/sadiyah-grobbler/)

Got feedback? A cool security tip? Or just want to share your latest IAM policy horror story? Reach out!

> *“May your deployments be green, your keys rotated, and your S3 buckets never public.”*  
