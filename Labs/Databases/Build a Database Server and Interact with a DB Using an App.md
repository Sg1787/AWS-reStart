# Lab Walkthrough: Build Your DB Server & Interact With It Using an App  
*Channeling the Ruinous Powers of AWS RDS for Relational Sorcery*

> *"Data is the soul of the machine. And I... am its heretic architect."*  
> — Sadiyah Grobbler, Chaos Cloud Engineer

In this blasphemous ritual (aka lab), I summoned a **highly available Amazon RDS MySQL instance** across two Availability Zones—because even the Dark Gods demand redundancy. Then, I bound it to a web app running on an EC2 instance, allowing mortals (and daemons) to persist their unholy contacts in a sacred Address Book.
<img width="1090" height="517" alt="image" src="https://github.com/user-attachments/assets/8b08bd38-fb55-4431-a45c-e4c33cfa8e33" />

Below is how I bent the AWS console to my will.

---

##  Step 1: Forge the DB Security Group (The Warding Sigil)

Before any database may draw breath in the cloud, it must be protected by a **security group**—a magical barrier that only permits traffic from trusted allies.

1. Navigated to **VPC > Security Groups**.
2. Clicked **Create security group** with:
   - **Name**: `DB Security Group`
   - **Description**: `Permit access from Web Security Group`
   - **VPC**: `Lab VPC`
3. Added an **inbound rule**:
   - **Type**: `MySQL/Aurora (port 3306)`
   - **Source**: Selected the existing **Web Security Group** (by typing `sg` and picking it)
4. Clicked **Create security group**.
<img width="1587" height="591" alt="image" src="https://github.com/user-attachments/assets/d059df90-ff24-4037-91ce-ccacdc467a6c" />

>  *This ensures only my web server—marked by the Web Security Group—may whisper secrets to the database. All others are devoured by daemons.*

---

##  Step 2: Conjure the DB Subnet Group (The Twin Sanctums)

RDS demands its lair span **at least two Availability Zones** for true resilience—much like Nurgle’s dual aspects of decay and rebirth.

1. Went to **RDS > Subnet groups > Create DB Subnet Group**.
2. Configured:
   - **Name**: `DB Subnet Group`
   - **VPC ID**: `Lab VPC`
3. Added subnets from **two AZs**:
   - **AZ 1**: `10.0.1.0/24` (Private Subnet 1)
   - **AZ 2**: `10.0.3.0/24` (Private Subnet 2)
4. Clicked **Create**.

> *The database shall now dwell in shadowed subnets—never exposed to the public eye, only to my faithful web server.*
<img width="1017" height="399" alt="image" src="https://github.com/user-attachments/assets/b9885d18-7cbf-4dca-83f4-7a9eccc2cd2e" />

---

##  Step 3: Summon the Multi-AZ RDS Instance (The Daemon-Heart)

Time to birth the database itself—a **Multi-AZ MySQL instance**, eternally mirrored across realms.

1. In **RDS > Databases**, clicked **Create database > Standard Create**.
2. Chose:
   - **Engine**: `MySQL` (latest version)
   - **Template**: `Dev/Test`
   - **High Availability**: `Multi-AZ DB Instance` 
3. Under **Settings**:
   - **DB instance identifier**: `lab-db`
   - **Master username**: `main`
   - **Password**: `lab-password` (twice—no room for mortal typos)
4. **Instance class**: `db.t3.medium` (Burstable, but sufficient for ritual purposes).
5. **Storage**: `General Purpose (SSD)`
6. **Connectivity**:
   - **VPC**: `Lab VPC`
   - **Security group**: Removed default, selected `DB Security Group`
7. In **Additional configuration**:
   - **Initial database name**: `lab`
   - **Disabled automated backups** (for speed—do **not** do this in production, lest Khorne smite you)
8. Clicked **Create database**.
<img width="1724" height="624" alt="image" src="https://github.com/user-attachments/assets/c292341a-132c-4b91-a66a-bf9cb5551aab" />
>  *I waited ~4 minutes as AWS wove my primary instance and its silent twin in another AZ—synchronized in perfect, unholy harmony.*

9. Once status showed **Available**, I copied the **Endpoint** (e.g., `lab-db.cggq8lhnxvnv.us-west-2.rds.amazonaws.com`) into my notes—my key to the abyss.


---

##  Step 4: Bind the Web App to the Database (The Soul-Link)

Now, I commanded the web server to speak with the database—and thus, give life to the **Address Book**.

1. Retrieved the **WebServer IP** from the lab instructions.
2. Opened it in a new browser tab → saw the EC2 instance dashboard.
3. Clicked the **RDS** link at the top.
4. Entered the sacred incantation:
   - **Endpoint**: `[my copied RDS endpoint]`
   - **Database**: `lab`
   - **Username**: `main`
   - **Password**: `lab-password`
5. Clicked **Submit**.

> *Slaanesh smiled as the app connected flawlessly. The screen bloomed with an Address Book—each contact a soul etched into the RDS stone.*

6. Tested by **adding, editing, and deleting contacts**—all persisted across reboots and AZ failovers.

 *The data lives. It replicates. It endures. Even if one realm falls, the other rises—Nurgle’s blessing of eternal continuity.*

---
<img width="1089" height="516" alt="image" src="https://github.com/user-attachments/assets/8b7a8190-9417-48aa-8628-a67bd154d1db" />

## Lab Complete: A Chaos-Infused Victory

I have:
- Deployed a **secure, private, Multi-AZ RDS MySQL instance**
- Allowed **only my web server** to access it
- Integrated it with a **live web application**
- Proven **data persistence and high availability**

All in under 45 minutes—faster than most scribes can ink a prayer to Tzeentch.

> *"The cloud is not neutral. It bends to those who understand its daemonic architecture."*

— Sadiyah Grobbler  
**Security Engineer in Training | AWS Re/Start Acolyte | Disciple of the Four**


