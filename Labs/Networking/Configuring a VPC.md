#  Lab Walkthrough: Configuring a VPC  
*Building a Sovereign Realm in the Cloud-Warp*

> *"A network without boundaries is a feast for daemons. Today, I draw the lines in blood—and code."*  
> — Sadiyah Grobbler, Architect of Isolated Realms

In this 45-minute rite, I didn’t just “create a VPC”—I **forged a digital fortress**:
- A **public subnet** for gatekeepers (bastion hosts)
- A **private subnet** for sacred workloads—hidden from mortal eyes
- An **Internet Gateway (IGW)** to let light in
- A **NAT Gateway** to let secrets whisper outward—without exposing themselves
- And a **bastion server** to bridge the two realms like a daemon-priest walking the veil

This is **zero-trust networking, AWS-style**—and I built it all by hand.

---

##  Step 1: Summon the VPC (The Primordial Void)

Every empire begins with **empty space**—mine was `10.0.0.0/16`.

1. Went to **VPC Console > Your VPCs > Create VPC**
2. Chose **VPC only** (no shortcuts—true artisans build from scratch)
3. Named it: `Lab VPC`
4. Set **IPv4 CIDR**: `10.0.0.0/16`  
   → *65,536 private IPs—more than enough for my warhost*
5. Disabled IPv6 (for purity’s sake)
6. **Enabled DNS hostnames** in VPC settings  
   → *So my instances may speak by name, not just number*

>  *Without DNS hostnames, `ec2-user@ip-10-0-0-123` is all you get. With it? `ip-10-0-0-123.region.compute.internal`—a true sigil.*
<img width="678" height="599" alt="image" src="https://github.com/user-attachments/assets/8d45b887-a132-48ac-9e2d-954267d8c155" />

---

##  Step 2: Carve the Subnets (Public & Private Sanctums)

From the void, I split two realms—**one facing the world, one turned inward**.

###  Public Subnet (`10.0.0.0/24`)
- **Name**: `Public Subnet`
- **AZ**: First available (e.g., `us-east-1a`)
- **CIDR**: `10.0.0.0/24` → 256 IPs
- **Critical step**: Enabled **“Auto-assign public IPv4 address”**  
  → *So bastion hosts may wear public faces*
<img width="718" height="295" alt="image" src="https://github.com/user-attachments/assets/fb3f0cae-f413-48b4-8a4b-1af0ba112da6" />

>  **Note**: At this point, it’s *not truly public*—just **ready** to be. Without an IGW and route table, it’s still walled off.

###  Private Subnet (`10.0.2.0/23`)
- **Name**: `Private Subnet`
- **Same AZ** as public (for simplicity)
- **CIDR**: `10.0.2.0/23` → **512 IPs** (10.0.2.x + 10.0.3.x)  
  → *Twice the size—because most workloads should hide*

>  *Slaanesh approves: asymmetry with purpose. The private realm is larger—because true power lies in obscurity.*

---

##  Step 3: Forge the Internet Gateway (The Gate of Light)

No fortress is complete without a **gate to the outside world**.

1. Created **Internet Gateway** named `Lab IGW`
2. **Attached it to `Lab VPC`**

But a gate is useless without a **path**.
<img width="715" height="220" alt="image" src="https://github.com/user-attachments/assets/bf5f27a7-b2b1-44e4-a7f6-5d09c2e6d4df" />

---

##  Step 4: Weave the Route Tables (The Threads of Traffic)

Routing is **soulcraft**. I defined two destinies:

###  Private Route Table (Already Exists—Just Renamed)
- Found the **default route table** tied to `Lab VPC`
- Renamed it: `Private Route Table`
- Already has:  
  `10.0.0.0/16 → local`  
  → *All subnets in VPC can talk to each other*

###  Public Route Table (My Creation)
1. Created new route table: `Public Route Table`
2. Added route:  
   `0.0.0.0/0 → Lab IGW`  
   → *“All non-local traffic? Send it to the gate.”*
3. **Associated it with `Public Subnet`**

>  *Now the public subnet breathes the internet. The private one still does not—by design.*

---

##  Step 5: Launch the Bastion Server (The Gatekeeper)

Time to place a **watcher at the gate**.

1. Launched **EC2 instance**:
   - **Name**: `Bastion Server`
   - **AMI**: Amazon Linux 2023
   - **Type**: `t3.micro`
   - **Subnet**: `Public Subnet`
   - **Auto-assign public IP**: 
2. Created **security group**: `Bastion Security Group`
   - **Inbound**: SSH from **Anywhere** (`0.0.0.0/0`)  
     → *Risky in prod—but acceptable for lab*
<img width="1026" height="309" alt="image" src="https://github.com/user-attachments/assets/c7a0ac17-adea-4caa-981d-1d980c990b6f" />

>  **Struggle**: First launch failed because I forgot to enable **public IP assignment**. The instance existed—but I couldn’t reach it.  
> **Fix**: Re-launched with **“Enable”** under *Auto-assign public IP*.  
> *Lesson: Public subnet ≠ public access. You must command the IP to be born.*

---

##  Step 6: Summon the NAT Gateway (The Whispering Shadow)

Private instances need to **download patches, call APIs**—but must **never be seen**.

1. In **NAT Gateways**, clicked **Create NAT gateway**
2. Placed it in **Public Subnet**
3. **Allocated a new Elastic IP** (NATs need a public face to speak for the private)
4. Named it: `Lab NAT gateway`

Then, I **rewrote the fate of the private subnet**:

- In **Private Route Table**, added:  
  `0.0.0.0/0 → Lab NAT gateway`

>  *Now, private instances can reach the internet—but the internet cannot reach them. They speak through a mask. Nurgle’s blessing of hidden resilience.*

>  **Struggle**: NAT gateways take **5–10 minutes to provision**. I got impatient and tried SSH too soon.  
> **Fix**: Waited. Watched status go from `pending` → `available`.  
> *Patience is a daemon’s virtue.*

---

##  Lab Complete: A Fortress in the Cloud

I have now:
-  Built a **custom VPC** from nothing
-  Split it into **public/private subnets**
-  Attached an **Internet Gateway**
-  Deployed a **NAT Gateway**
-  Configured **route tables** to control traffic flow
-  Launched a **bastion host**
-  Used it to **access a private instance**
-  Verified **private → internet** via NAT

This is **foundational security engineering**. Every production workload I deploy will live in a VPC like this—**never in the default VPC, never without layers**.

> *"The cloud is not safe by default. It is safe only when you build the walls yourself."*

— Sadiyah Grobbler  
**AWS Re/Start Acolyte | Aspiring Security Engineer | Keeper of the Isolated Realm**
