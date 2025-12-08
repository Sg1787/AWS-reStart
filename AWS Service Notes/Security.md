#  AWS Security Notes  
## *“Security is not cleanliness—it is controlled decay.”*  
> — **Grandfather Nurgle**, while enabling MFA on His Root Account

Welcome, mortal guardian of the cloud!  
In the grand garden of AWS, **threats bloom like mold**—but you? You shall tend your systems with **wisdom, encryption, and well-configured Security Groups**.

This guide covers the **full life cycle of security**—from prevention to post-rot analysis—all in plain English, with a side of divine pestilence.

---

##  The Nine Plagues of AWS Security (aka Services)

| Service | Nurgle’s Whisper | Real Purpose | Domain |
|--------|------------------|--------------|--------|
| **IAM** | *"Who may touch the sacred mold?"* | Manage users, roles, and permissions | Identity |
| **CloudTrail** | *"Every sin is recorded in the Book of Rot."* | Logs all API calls for auditing | Detection |
| **AWS Config** | *"Is your bucket still pure… or has it bloomed?"* | Checks config compliance (e.g., “Is S3 public?!”) | Compliance |
| **Trusted Advisor** | *"The Oracle of Mildew"* | Gives real-time tips on security, cost, and health | Best Practices |
| **ACM** | *"Blessings for encrypted tongues"* | Free, auto-renewing SSL/TLS certs | PKI / Encryption |
| **KMS** | *"The Key Keeper of the Crypt"* | Manages encryption keys for data at rest | Data Security |
| **WAF** | *"The Wall of Boils"* | Blocks web attacks (SQLi, XSS) at the gate | Network Hardening |
| **Inspector** | *"The Soul-Sniffer"* | Scans EC2 for vulnerabilities & rotten packages | Detection |

> *"A system without logs is a corpse with no name."*

---

## 1.  The Shared Burden of Rot

### The **Shared Responsibility Model**
- **AWS** secures the **"Security *of* the Cloud"**:  
  → Physical data centers, hypervisors, global network.  
  → *“I guard the cauldron,” says Nurgle.*

- **YOU** secure the **"Security *in* the Cloud"**:  
  → IAM, data, OS patches, network rules, app configs.  
  → *“You tend the stew,” says Nurgle. “Don’t let it spoil.”*

### Acceptable Use Policy
- **No summoning of unauthorized daemons** (e.g., malware, cryptojacking, illegal pentesting).  
- AWS will **ban your soul** if you spread *bad* rot.

> *"Even Grandfather Nurgle follows the rules… mostly."*

---

## 2.  Prevention: Build a Rot-Resistant Garden

###  Identity Management (IAM)
- **Principle of Least Privilege**:  
  → Don’t give a Plaguebearer `AdministratorAccess` just to water the roses.  
  → Use **roles**, **groups**, and **minimal policies**.

###  Public Key Infrastructure (PKI)
- **ACM** gives you **free, auto-renewing SSL certs**.  
  → No more expired certs causing browser screams!  
  → *“Let all traffic be wrapped in blessed encryption.”*

###  Network Hardening
- **VPC** = your walled garden.  
- **Security Groups** = bouncers at each server door (allow only trusted guests).  
- **NACLs** = neighborhood watch (subnet-level).  
- **WAF** = the spell that blocks cursed web requests.

###  System Hardening
- **Patch your OS** like you’d clean a wound.  
- Use **AWS-provided AMIs**—they’re pre-sanitized!  
- Disable unused services. *Fewer open ports = fewer entry points for chaos.*

###  Data Security
- **At rest?** → Encrypt with **KMS** or S3 server-side encryption.  
- **In transit?** → Enforce **TLS** (via ACM certs).  
- *“A secret not encrypted is a secret already lost.”*

---

## 3.  Detection & Response: When the Mold Blooms

###  Detection Tools
- **CloudTrail**: “Who deleted the database at 3 AM?” → *This log knows.*  
- **Config**: “Is this S3 bucket public?!” → *It screams if so.*  
- **GuardDuty** (bonus plague!): AI-powered threat detector.  
  → Finds crypto miners, port scanners, and rogue logins.

###  Response
- **Isolate** the infected instance.  
- **Revoke** compromised keys.  
- **Rotate** secrets like you’d burn a contaminated robe.

###  Analysis
- Use **CloudTrail + VPC Flow Logs** to retrace the attacker’s slimy footsteps.  
- Ask: *“How did the rot enter? How do we seal the crack?”*

###  Monitoring EC2
- **CloudWatch**: Tracks CPU, memory, disk—your server’s vital signs.  
- **Inspector**: Runs weekly scans for vulnerable packages.  
  → *“Even the healthiest host grows mold in silence.”*

---

## 4.  Compliance & Eternal Best Practices

###  Trusted Advisor
- Your **free security advisor**.  
- Checks:  
  - Is **MFA enabled** on root?  
  - Are **Security Groups** wide open?  
  - Are you **hitting service limits**?

###  Top 4 Security Commandments
1. **Enable MFA on root**. No excuses.  
2. **Never use root for daily work**. Create an IAM user.  
3. **Least privilege always**.  
4. **Encrypt everything sensitive**—at rest *and* in transit.

###  AWS Compliance
- AWS is **SOC 2, HIPAA, PCI-DSS compliant** (for their layer).  
- Use **AWS Artifact** to download audit reports for *your* compliance needs.

###  More Security Blessings
- **AWS Security Hub**: Central view of your security posture.  
- **Macie**: Finds sensitive data (like PII) hiding in S3.  
- **Well-Architected Framework (Security Pillar)**: Your path to enlightenment.

---

##  Final Wisdom from the Grandfather

> *“Security is not about perfection—it’s about visibility, response, and learning from every spill, leak, and outbreak.”*

So:
- **Prevent** with least privilege and encryption.  
- **Detect** with logs and alerts.  
- **Respond** fast.  
- **Document** everything.  

And remember:  
> *“A well-monitored system doesn’t fear decay—it expects it, contains it, and grows stronger from it.”*

 **May your keys be rotated, your buckets be private, and your alerts be loud.**  
*— The Plaguebearers of AWS Security*
