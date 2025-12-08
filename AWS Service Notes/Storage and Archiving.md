#  AWS Storage & Archiving: The Grandfather’s Guide to Sacred Data Preservation  
## *“A byte unarchived is a soul unremembered.”*  
> — **Grandfather Nurgle**, while compressing His 10,000-year plague ledger into Glacier Deep Archive

Welcome, Keeper of the Eternal Scrolls!  
In AWS, your data isn’t just stored—it’s **honored, protected, and layered like the rings of a sacred onion**.  
This guide covers **disks, file systems, object stores, and time capsules**—so your data **rots gracefully**, **scales endlessly**, and **never truly dies**.

All hail the **Four Temples of Storage**:  
`Block` • `File` • `Object` • `Archive`

---

## 1. Storage & Archiving Overview: The Hierarchy of Decay

| Type | Use Case | AWS Service | Nurgle’s Verdict |
|------|--------|------------|------------------|
| **Block Storage** | Raw disks for EC2 (like a golem’s heart) | **Amazon EBS** | *“Fast, persistent, and tied to one host.”* |
| **File Storage** | Shared file system for many golems | **Amazon EFS** | *“One scroll, read by a thousand eyes.”* |
| **Object Storage** | Infinite, durable buckets for anything | **Amazon S3** | *“The bottomless chest that never loses a byte.”* |
| **Archive Storage** | Long-term cold storage (decades!) | **S3 Glacier** | *“For secrets too precious to delete, too stale to keep warm.”* |

>  *"EBS dies with the instance. S3 outlives empires."*

---

##  2. Amazon EBS: The Golem’s Heart

**EBS** = **Elastic Block Store** → virtual hard drives for your EC2 instances.

###  Key Facts
- **Persistent**: Lives beyond instance stop/start.
- **AZ-scoped**: Tied to one Availability Zone.
- **Types**: 
  - `gp3` (general purpose, SSD)
  - `io2` (high-performance, mission-critical)
  - `st1`/`sc1` (cheap, for logs & backups)

### Create & Attach an EBS Volume (CLI)
```bash
# Create a 100 GiB gp3 volume
aws ec2 create-volume --size 100 --availability-zone us-east-1a --volume-type gp3

# Attach to an instance
aws ec2 attach-volume --volume-id vol-1234567890abcdef0 --instance-id i-1234567890abcdef0 --device /dev/sdf
```

>  *"An EBS volume is a loyal servant—faithful to one master, fast in battle, but fragile if the realm falls."*

---

##  3. Instance Store: The Golem’s Short-Term Memory

- **Ephemeral storage** directly on the EC2 host.
- **Blazing fast** (NVMe SSDs), but **wiped** on stop/terminate.
- Use for **caching**, **scratch space**, or temporary processing.

>  *"Instance Store is like a fever dream—intense, fast, and gone by morning."*

---

##  4. Amazon EFS: The Shared Scroll Room

**EFS** = **Elastic File System** → NFS file system shared across **hundreds of EC2 instances**.

###  When to Use
- Web servers sharing code/assets
- Media processing pipelines
- Home directories for HPC clusters

###  Mount EFS on Linux
```bash
# Install NFS client
sudo yum install -y amazon-efs-utils

# Mount
sudo mkdir /mnt/efs
sudo mount -t efs fs-12345678:/ /mnt/efs
```

>  *"EFS is the communal soup pot—everyone stirs, no one owns, all are fed."*

---

##  5. Storage with Amazon S3: The Bottomless Chest

**S3** = **Simple Storage Service** → object storage with **11 nines (99.999999999%) durability**.

###  Core Concepts
- **Buckets** = top-level containers (globally unique names!)
- **Objects** = files + metadata (max 5 TB)
- **Storage Classes**:
  - `S3 Standard` → hot data
  - `S3 Intelligent-Tiering` → auto-moves based on access
  - `S3 Glacier Instant/Deep Archive` → cold/colder data

### Common S3 CLI Commands
```bash
# Create a bucket
aws s3 mb s3://my-plague-archive-2025

# Upload a file
aws s3 cp report.csv s3://my-plague-archive-2025/

# Sync a whole folder
aws s3 sync ./data/ s3://my-plague-archive-2025/data/ --delete

# List contents
aws s3 ls s3://my-plague-archive-2025/
```

>  *"S3 is the ocean—vast, eternal, and indifferent to your puny files."*

---

##  6. Amazon S3 Glacier: The Time Capsule of Rot

For data you **must keep but won’t touch for years**.

| Tier | Retrieval Time | Cost | Nurgle’s Use |
|------|---------------|------|-------------|
| **Glacier Instant** | 1–5 min | $$$ | “Oops, need it now!” |
| **Glacier Flexible** | 1–5 min (expedited), 3–5 hrs (standard) | $$ | “Plague research backups” |
| **Glacier Deep Archive** | 12 hrs | $ | “Ancient scrolls of forgotten blights” |

###  Archive to Glacier via S3 Lifecycle
```bash
# Move objects to Glacier after 90 days
aws s3api put-bucket-lifecycle-configuration \
  --bucket my-plague-archive-2025 \
  --lifecycle-configuration file://lifecycle.json
```

`lifecycle.json`:
```json
{
  "Rules": [{
    "ID": "ArchiveOldData",
    "Status": "Enabled",
    "Filter": {"Prefix": "logs/"},
    "Transitions": [{
      "Days": 90,
      "StorageClass": "GLACIER"
    }]
  }]
}
```

>  *"Glacier is not deletion—it is patience."*

---

##  7. AWS Storage Gateway: Bridging Realms

**Storage Gateway** lets on-prem apps **seamlessly use AWS storage**.

###  Three Flavors
| Type | Use Case | How It Works |
|------|--------|-------------|
| **File Gateway** | NFS/SMB file shares backed by S3 | On-prem sees a file share → data lives in S3 |
| **Volume Gateway** | Block storage backed by EBS/S3 | iSCSI volumes → stored in AWS |
| **Tape Gateway** | Virtual tape library → Glacier | Replace physical tapes with virtual ones |

>  *"Storage Gateway is the ferryman—carrying your data across the river Styx to the cloud."*

---

##  8. AWS Transfer Family & Migration Services

###  AWS Transfer Family
- Fully managed **SFTP, FTPS, FTP** service.
- Files land directly in **S3 or EFS**.
- Perfect for partners who “still use FTP” (bless their hearts).

```bash
# Create an SFTP server
aws transfer create-server --protocols SFTP --identity-provider-type SERVICE_MANAGED
```

###  Other Migration Tools
- **AWS DataSync**: Fast, secure sync between on-prem/NFS → S3/EFS.
- **Snowball**: Physical device for **petabyte-scale** offline transfers.

>  *"Not all mortals speak REST. Some still whisper in FTP."*

---

##  Final Blessing from the Grandfather

> *“Store not for convenience—but for eternity.  
> Let hot data burn bright, cold data sleep deep, and shared data feed the many.”*

So:
- Use **EBS** for EC2 boot volumes.
- Use **EFS** for shared file systems.
- Use **S3** for everything else (it’s durable, cheap, and eternal).
- Use **Glacier** for data you love but won’t miss for a decade.
- And **never** store critical data only on Instance Store.

 **May your buckets be versioned, your backups be encrypted, and your archives never thaw prematurely.**  
*— The Plaguebearers of AWS Storage*

>  *Inspired by real AWS services. No actual plagues were lost to bit rot (yet).*
