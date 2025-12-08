# AWS Networking Fundamentals  
## *“A well-rotted network is a well-defended one.”*  
> — **Grandfather Nurgle**, while subnetting His latest daemon swarm

Welcome, weary gardener of the cloud!  
In AWS, your network is not just wires and IPs—it’s a **living ecosystem**. And like all living things, it will **decay**… unless you tend to it with wisdom, firewalls, and a healthy dose of divine pestilence.

Let us walk through the **sacred layers of your VPC**, guided by the eternal truths of the Plague Lord.

---

## 1. The Sacred Flesh: IP Addresses & Subnets

### IPv4 vs. IPv6: The Finite vs. The Infinite
- **IPv4** = 4 billion addresses. Like clean water in a drought—**precious, limited, and already half-gone**.
- **IPv6** = 340 undecillion addresses. **More than there are stars**—or boils on Nurgle’s back.  
  → *“Use IPv6 if you dare to dream of endless rot.”*

### VPC IPs: Your Private Garden
- All VPCs use **private IP ranges**: `10.0.0.0/8`, `172.16.0.0/12`, or `192.168.0.0/16`.  
  → *“The outside world shall not sully your inner sanctum.”*

### Subnetting: Carving the Corpse
- A **/24 subnet** = 256 addresses.  
- But AWS **reserves 5** for itself (router, DNS, future horrors…).  
  → You get **251 usable IPs**.  
  → *“Even the Grandfather leaves room for His servants.”*

### Public vs. Private: Two Worlds
- **Private IP**: Lives inside your VPC. Safe. Pure. Untouched by the screaming internet.
- **Public IP**: Fleeting—changes when you stop/start an instance.  
- **Elastic IP (EIP)**: A **permanent public face** for your most important golems.  
  → *“Use EIPs for things that must not lose their name.”*

### DHCP: The Automatic Sower
- AWS **auto-assigns private IPs** when you launch an EC2.  
- Controlled by **DHCP Option Sets**—your network’s “birth registry.”  
  → *“Every golem deserves an address at birth.”*

---

## 2. The Walled Garden: Your VPC

Your **VPC** is your **sanctified realm**—a bubble of order in the screaming chaos of the cloud.

### Core Components (The Divine Trinity)
| Piece | What It Is | Nurgle’s Wisdom |
|------|-----------|----------------|
| **VPC** | Your network boundary (e.g., `10.0.0.0/16`) | *“Define your domain, or Chaos will define it for you.”* |
| **Subnet** | A zone inside your VPC (one per Availability Zone) | *“Spread your rot across realms—never put all maggots in one jar.”* |
| **Internet Gateway (IGW)** | Door to the outside world | *“Open it only for public subnets. Keep private things *private*.”* |

### NAT Gateway: The Hidden Messenger
- Lives in a **public subnet**.  
- Lets **private instances** reach the internet (for patches, updates, etc.)—**without exposing them**.  
  → *“Even the shyest database deserves fresh rot.”*

### The Journey of a Packet (Inbound Request)
1. Packet knocks on the **IGW** (the outer gate).
2. **Route Table** asks: “Is this for us?” → sends to `local` or `0.0.0.0/0`.
3. **NACL** judges: “Is this packet pure?” (stateless firewall).
4. **Security Group** whispers: “Do I know this caller?” (stateful guardian).
5. If **all say yes**—the packet enters your instance.

>  *"A packet that passes all tests is worthy of your rot."*

---

## 3.  Two Layers of Filth: Firewalls That Love You

### Security Groups (SGs): Your Personal Bodyguards
- **Per resource** (EC2, RDS, ALB).
- **Allow-only**. No rule = **DENIED**.
- **Stateful**: If you allow **inbound 443**, replies flow freely.
- **Best Practice**: Reference **other SGs**, not IPs!  
  → *“Allow web-tier to talk to db-tier”* → clean, dynamic, eternal.

### Network ACLs (NACLs): The Subnet Judges
- **Per subnet** (not per instance!).
- **Allow + Deny**, processed by **rule number** (Rule 10 beats 100).
- **Stateless**: Allow **inbound 80**? You **must allow outbound 1024-65535** for replies!
- **Use Case**: Ban a malicious IP with a `DENY` rule at the top.

> *"SGs say ‘Who are you?’ NACLs say ‘You shall not pass!’"*

---

## 4. Spreading the Rot: Global Connectivity

### Load Balancers: The Plaguebearers’ Gatekeepers
| Type | Layer | When to Use |
|------|-------|------------|
| **ALB** | L7 (HTTP/HTTPS) | “Is the *app* alive?” → `/healthcheck` |
| **NLB** | L4 (TCP) | “Send traffic **FAST**—like a daemon sprinting.” |
| **Global Accelerator** | L3/L4 | “Route users to the **closest healthy rot** using AWS’s global backbone.” |

###  Route 53: The Oracle of Names
- **DNS that never sleeps**.  
- Use **Alias Records** to point `myapp.com` → ALB/S3 **without IP addresses**.  
  → *“Names are more eternal than numbers.”*

### VPC Endpoints: Secret Tunnels to AWS
- Connect **privately** to S3, DynamoDB, etc.—**no internet, no NAT, no IGW**.  
- Data **never leaves AWS network**.  
  → *“Even secrets must be kept from the eyes of Chaos.”*

### Bridging Realms: Hybrid Networks
- **Transit Gateway (TGW)**: The **central rot-spreader**.  
  → Connect 100 VPCs + on-prem with **one hub** (no messy peering!).  
- **Direct Connect**: A **private cable** to AWS (for the truly paranoid).  
- **Site-to-Site VPN**: Encrypted tunnel over internet (cheaper, faster to deploy).

---

## 5. Watching the Rot: Security & Compliance

| Tool | What It Does | Nurgle’s Whisper |
|------|-------------|------------------|
| **VPC Flow Logs** | Logs all traffic (who talked to whom) | *“Record every breath—so you may know who betrayed you.”* |
| **AWS WAF** | Blocks web attacks (SQLi, XSS) | *“Let no cursed script enter your ALB.”* |
| **AWS Artifact** | Gives you compliance reports (SOC, PCI) | *“Here are the holy scrolls to pacify auditors.”* |
| **Amazon Inspector** | Scans EC2 for vulnerable packages | *“Find the rotten code before Chaos finds it.”* |

---

## Final Blessing from the Grandfather

> *“Build not for perfection—build for decay.  
> Let your networks breathe, heal, and grow stronger from every breach.”*

So:
- **Keep things private** by default.
- **Layer your defenses** (SGs + NACLs).
- **Never expose databases to the internet**.
- **Automate, monitor, and document**—even your failures.

**May your subnets be tight, your routes be clear, and your Elastic IPs never float away.**  
*— The Plaguebearers of AWS Networking*
