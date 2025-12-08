#  AWS Cost & Resource Management: The Grandfather’s Guide to Frugal Rot  
## *“Even plagues must budget their pus.”*  
> — **Grandfather Nurgle**, while deleting 47 idle t3.micro instances and enabling S3 Intelligent-Tiering

Welcome, Keeper of the Golden Mold!  
In AWS, **rot is free—but resources are not**.  
This guide teaches you to **track, tag, trim, and optimize** your cloud garden—so you **spend gold only on what truly blooms**.

All hail the **Three Pillars of Divine Frugality**:  
`Tag` • `Track` • `Trim`

---

##  1. Manage Resource Consumption Overview: Know Thy Rot

Before you can optimize, you must **see** what you’re paying for.

>  *"An untagged resource is a forgotten golem—still eating gold, long after its purpose is gone."*

---

##  2. AWS Organizations: The Plague Empire

**AWS Organizations** lets you manage **multiple AWS accounts** under one umbrella—perfect for **dev, test, prod**, or **business units**.

###  Key Features
- **Consolidated Billing**: One bill for all accounts (volume discounts!).
- **Service Control Policies (SCPs)**: Restrict what accounts can do (e.g., “No root user actions”).
- **Organizational Units (OUs)**: Group accounts (e.g., “Security”, “Marketing”).

###  Create an Organization (CLI)
```bash
# Enable Organizations (only in management account)
aws organizations create-organization --feature-set ALL
```

>  *"One empire, many realms—but all bow to the same ledger."*

---

##  3. Tagging: The Sacred Naming Ritual

**Tags** = key/value pairs (`Environment: prod`, `Owner: nurgle`) attached to resources.

###  Why Tag?
- **Cost Allocation**: See who spends what (via **Cost Explorer**).
- **Automation**: Auto-stop dev instances at night (`AutoStop: true`).
- **Compliance**: Enforce naming standards.

###  Tag an EC2 Instance (CLI)
```bash
aws ec2 create-tags \
  --resources i-1234567890abcdef0 \
  --tags Key=Project,Value=PlagueSpread Key=Owner,Value=Nurgle Key=Environment,Value=prod
```

>  *"A resource without tags is an orphan—and orphans cost gold."*

---

##  4. AWS Billing Dashboard & Cost Explorer: The Oracle of Gold

###  What You’ll See
- **Daily/monthly spend**
- **Top services** (EC2? S3? Lambda?)
- **Cost by tag** (e.g., “How much does Team Chaos spend?”)

###  Enable Cost & Usage Reports (for deep dives)
1. Go to **AWS Billing Console → Cost & Usage Reports**
2. Create report → Deliver to **S3 bucket**
3. Query with **Athena** or **QuickSight**

>  *"The Billing Dashboard doesn’t lie—but it won’t tell you how to save, either."*

---

##  5. AWS Cost Management & Best Practices: Trim the Fat

###  Nurgle’s Cost-Saving Commandments

| Practice | How It Saves | CLI/Tool |
|--------|-------------|--------|
| **Delete idle resources** | No charge for deleted EC2, EBS, IPs | `aws ec2 terminate-instances --instance-ids i-...` |
| **Use S3 Intelligent-Tiering** | Auto-moves objects to cheaper tiers | Set on bucket creation |
| **Right-size EC2** | Don’t run `m5.4xlarge` for a blog | Use **Trusted Advisor** or **Compute Optimizer** |
| **Use Spot Instances** | Up to 90% off for fault-tolerant workloads | `--instance-market-options "MarketType=spot"` |
| **Reserve Instances / Savings Plans** | 1–3 year commitment = big discounts | Buy in **AWS Cost Management Console** |
| **Enable auto-shutdown for dev** | Stop non-prod instances nights/weekends | Use **Instance Scheduler** or **Lambda** |

###  Example: Auto-Stop Dev Instances (Lambda Snippet)
```python
import boto3
ec2 = boto3.client('ec2')
def lambda_handler(event, context):
    # Stop all instances tagged AutoStop=true
    instances = ec2.describe_instances(
        Filters=[{'Name': 'tag:AutoStop', 'Values': ['true']}]
    )
    ids = [i['InstanceId'] for r in instances['Reservations'] for i in r['Instances']]
    ec2.stop_instances(InstanceIds=ids)
```

>  *"Gold wasted on idle golems is gold stolen from new plagues."*

---

##  6. AWS Support Services: Call the Healers

| Plan | Best For | Nurgle’s Take |
|------|--------|--------------|
| **Basic (Free)** | General guidance, health checks | *“For mortals who fear cost more than outages.”* |
| **Developer** | Business hours, <24h response | *“For hobbyists and dreamers.”* |
| **Business** | 24/7, <4h response, TAM access | *“For realms that must not fall.”* |
| **Enterprise** | 15-min response, dedicated team | *“For empires that bleed gold to stay alive.”* |

>  *"Support isn’t a cost—it’s insurance against screaming at 3 AM."*

---

##  7. Café Activity: Optimizing AWS Resource Utilization

> *“Your challenge: Find and fix waste in a mock AWS account.”*

###  Common Waste Patterns
- **EC2 instances running 24/7** (but only used 9–5)
- **Unattached EBS volumes** (orphaned disks)
- **S3 buckets with no lifecycle rules**
- **RDS instances oversized for workload**

### Optimization Checklist
**Tag everything**  
**Run Trusted Advisor checks** (under "Cost Optimization")  
**Enable AWS Compute Optimizer**  
**Set billing alerts** (`aws budgets create-budget ...`)  
**Delete what you don’t need—ruthlessly**

>  *"Optimization isn’t a project—it’s a daily ritual of pruning."*

---

##  Final Blessing from the Grandfather

> *“Spend not like a king—but like a gardener:  
> nourish what grows, prune what withers, and let nothing rot in silence.”*

So:
- **Tag all resources**.
- **Review bills weekly**.
- **Delete the dead weight**.
- **Automate savings**.
- And **never pay for idle golems again**.

**May your cost curves slope downward, your savings plans be wise, and your finance team smile upon you.**  
*— The Plaguebearers of AWS Cost Optimization*

> *Inspired by real AWS services. No actual gold was wasted (yet).*
