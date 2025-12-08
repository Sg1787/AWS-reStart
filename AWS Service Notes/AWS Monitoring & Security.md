# AWS Monitoring & Security: The Grandfather’s Guide to Watching the Rot  
## *“A system that isn’t watched is a system already dying.”*  
> — **Grandfather Nurgle**, while setting a CloudWatch alarm for “Too Few Boils”

Welcome, Keeper of the Eternal Vigil!  
In AWS, your systems **will decay**—but with the right eyes and ears, you’ll **see it coming**, **heal it fast**, and **learn from the rot**.

This guide covers **CloudWatch**, **CloudTrail**, and **Athena**—your holy trinity of **visibility, audit, and wisdom**.

All hail the **Three Eyes of Nurgle**:  
`Observe` • `Record` • `Understand`

---

##  1. Monitoring & Security Overview: The Divine Watch

| Service | What It Does | Nurgle’s Whisper |
|--------|-------------|------------------|
| **Amazon CloudWatch** | Monitors **metrics**, **logs**, and **sets alarms** | *“Let your systems scream before they die.”* |
| **AWS CloudTrail** | Logs **every API call**—who did what, when, and where | *“Every sin is recorded in the Book of Rot.”* |
| **Amazon Athena** | Query logs **with SQL**—turn noise into insight | *“Ask the dead what killed them.”* |

>  *"If you aren’t logging, you’re flying blind through a plague storm."*

---

##  2. Amazon CloudWatch: The All-Seeing Pustule

**CloudWatch** is your **central nervous system** for AWS—tracking health, performance, and screams of pain.

###  Core Features

| Feature | Use Case | CLI Example |
|--------|--------|------------|
| **Metrics** | CPU, memory, network, custom app metrics | Built-in—no setup needed! |
| **Logs** | Stream app & system logs from EC2, Lambda, etc. | ```bash aws logs create-log-group --log-group-name "/plague/app"``` |
| **Dashboards** | Visualize key metrics in one place | Create in AWS Console |
| **Alarms** | Trigger actions when thresholds are crossed | 
```bash
Alarms cli Example
  aws cloudwatch put-metric-alarm \
  --alarm-name "PlagueCPUHigh" \
  --alarm-description "CPU > 80% for 5 mins" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 1
```

###  Pro Tip: Use **Embedded Metric Format (EMF)**
Your app logs can **auto-create CloudWatch metrics**—no extra code!

>  *"An alarm unheeded is a plague ignored."*

---

##  3. Deep Dive: Amazon CloudWatch Logs

###  Common Log Sources
- **EC2**: App logs, system logs (`/var/log`)
- **Lambda**: `console.log()` → CloudWatch Logs
- **RDS**: Database error logs
- **VPC Flow Logs**: Who talked to whom (and who got blocked)

###  Query Logs with **CloudWatch Logs Insights**

```sql
-- Find all 5xx errors in last hour
fields @timestamp, @message
| filter status >= 500
| sort @timestamp desc
| limit 20
```

>  *"Logs are not just records—they are prophecies of failure."*

---

##  4. AWS CloudTrail: The Book of All Sins

**CloudTrail** answers:  
> *“Who deleted the S3 bucket at 3 AM?”*

### 🧪 What It Captures
- Every **API call** in your account (even console actions!)
- **User**, **timestamp**, **IP address**, **parameters**
- Stored in **S3** (and optionally **CloudWatch Logs**)

###  Enable CloudTrail (CLI)
```bash
aws cloudtrail create-trail \
  --name MyPlagueTrail \
  --s3-bucket-name my-plague-logs-bucket \
  --is-multi-region-trail \
  --enable-log-file-validation
```

>  *"CloudTrail doesn’t judge—it remembers."*

---

##  5. AWS Service Integration with Athena: Ask the Dead

**Athena** lets you **query CloudTrail, VPC Flow Logs, and more** using **plain SQL**—no servers, no setup.

###  Why Use Athena?
- Find **malicious IPs** in CloudTrail
- Analyze **traffic patterns** in VPC Flow Logs
- Answer: *“How often do we scale?”* or *“Who’s scanning our ports?”*

###  Query CloudTrail Logs with Athena

1. **Point Athena to your CloudTrail S3 bucket**
2. **Run SQL like this**:
```sql
-- Who tried to delete S3 buckets?
SELECT eventname, sourceipaddress, useragent
FROM cloudtrail_logs
WHERE eventname LIKE '%DeleteBucket%'
  AND errorcode IS NULL;
```

3. **Save queries** as “Plague Investigations”

>  *"Athena turns log noise into divine insight—one SELECT at a time."*

---

##  Bonus: Nurgle’s Monitoring Checklist

**Enable CloudTrail in all regions** (use multi-region trail)  
**Turn on VPC Flow Logs** (for security forensics)  
**Set CloudWatch Alarms** for:
- High CPU
- Low disk space
- Lambda errors
- RDS storage full  
**Use Athena** to investigate anomalies  
**Never** assume “it’s fine”—**verify with data**

>  *"The wise admin doesn’t prevent all failures—they ensure no failure goes unnoticed."*

---

##  Final Blessing from the Grandfather

> *“Watch. Record. Understand.  
> Let every log be a lesson, every alarm a teacher, and every breach a path to greater resilience.”*

So:
- **Log everything** (you’ll thank yourself later).
- **Alert on symptoms**, not just outages.
- **Query logs like a detective**—not a janitor.
- And **assume breach**—because Chaos is always watching.

 **May your dashboards be green, your alerts be loud, and your post-mortems be blameless.**  
*— The Plaguebearers of AWS Observability*

>  *Inspired by real AWS services. No actual plagues were hidden in your logs (…probably).*
