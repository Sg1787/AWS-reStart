# AWS Servers & Hosting: A Plaguebearer’s Guide to Digital Flesh  
## *“A server that isn’t rotting is a server that isn’t working.”*  
> — **Grandfather Nurgle**, while deploying His 10,000th S3 bucket

Welcome, Keeper of Golems and Gardener of Static Sites!  
In AWS, you don’t just *run* servers—you **cultivate them**. And sometimes, you skip servers altogether and let the cloud **grow your website like mold on blessed bread**.

This guide covers everything from **EC2 flesh-puppets** to **serverless shrines**—all through the eyes of the Divine Rot.

---

##  1. Servers Overview: Flesh, Cloud, or Nothing?

You have **three paths** to host your digital soul:

| Path | What It Is | When to Use | Nurgle’s Verdict |
|------|-----------|------------|------------------|
| **EC2** | Virtual servers you control (OS, patches, configs). | You need full control (e.g., custom apps, legacy systems). | *“Flesh that computes. Tend it well.”* |
| **Elastic Beanstalk** | AWS manages the servers, OS, and scaling—**you just give code**. | Web apps (Java, Python, .NET) with minimal ops overhead. | *“Let AWS tend the golems—you tend the logic.”* |
| **S3 (Static Site)** | **No server needed!** Just HTML, CSS, JS in a bucket. | Brochure sites, portfolios, docs, landing pages. | *“Pure digital mold—simple, eternal, and free.”* |

>  *"The best server is the one you don’t have to patch."*

---

## 2. Hosting a Static Website on Amazon S3: The Serverless Shrine

Why run a server when you can **bless a bucket**?

###  Step-by-Step (The Sacred Ritual)

```bash
# 1. Create a bucket (name must be globally unique!)
aws s3 mb s3://my-plague-website-2025

# 2. Enable static website hosting
aws s3 website s3://my-plague-website-2025 --index-document index.html

# 3. Upload your files
aws s3 sync ./my-website/ s3://my-plague-website-2025 --acl public-read

# 4. Visit your site!
# URL format: http://my-plague-website-2025.s3-website-us-east-1.amazonaws.com
```

###  Critical Permissions (Bucket Policy)
Your bucket **must allow public read** for the site to work:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::my-plague-website-2025/*"
  }]
}
```

>  *"A static site is like a tomb—simple, quiet, and forever."*

---

##  3. Computing on AWS: The Many Forms of Flesh

| Service | Use Case | Nurgle’s Whisper |
|--------|--------|------------------|
| **EC2** | Full control over virtual servers. | *“Your golem, your rules.”* |
| **Lambda** | Run code without servers (event-driven). | *“Spirits that act, then vanish.”* |
| **Fargate** | Run containers without managing servers. | *“Swarm-bearers in a jar.”* |
| **Elastic Beanstalk** | Deploy apps without managing infra. | *“Drop code, get website.”* |

>  *"Choose your rot wisely: control vs. convenience."*

---

##  4. Managing AWS Instances (EC2): Tending Your Golems

###  Common EC2 Commands

```bash
# Launch an instance (Amazon Linux 2023)
aws ec2 run-instances \
  --image-id ami-0abcdef1234567890 \
  --instance-type t3.micro \
  --key-name my-key \
  --security-group-ids sg-12345678

# Stop an instance
aws ec2 stop-instances --instance-ids i-1234567890abcdef0

# Start it again
aws ec2 start-instances --instance-ids i-1234567890abcdef0

# Terminate (permanent!)
aws ec2 terminate-instances --instance-ids i-1234567890abcdef0
```

>  *"An idle instance is just an expensive paperweight. Turn it off!"*

---

##  5. AWS Elastic Beanstalk: The Lazy Admin’s Dream

Deploy a web app **without writing a single CloudFormation template**.

###  How It Works
1. Write your app (e.g., `app.py`).
2. Zip it or use `eb deploy`.
3. Elastic Beanstalk:
   - Provisions EC2 + Load Balancer + Auto Scaling
   - Deploys your code
   - Handles health checks & logs

###  Why Use It?
- **Zero infra code** for simple apps.
- Built-in **scaling**, **monitoring**, and **rollback**.
- Perfect for **dev, test, or small prod apps**.

>  *"Beanstalk is for mortals who want to ship—not babysit servers."*

---

##  6. Troubleshooting EC2 Creation: When the Golem Won’t Rise

Even the Grandfather’s golems sometimes **refuse to awaken**. Here’s how to heal them:

###  Common Issues & Cures

| Symptom | Likely Cause | Fix |
|--------|-------------|-----|
| **“Pending” forever** | - No free IP in subnet<br>- Missing IAM role for SSM | - Check subnet IP availability<br>- Attach `AmazonSSMManagedInstanceCore` role |
| **SSH timeout** | - Security Group blocks port 22<br>- No public IP | - Open port 22 from your IP<br>- Enable “Auto-assign public IP” |
| **“InsufficientInstanceCapacity”** | - Too many `t3.micro` in AZ | - Try a different AZ or instance type |
| **“Client.InstanceInitiatedShutdown”** | - User data script failed | - Check **EC2 Serial Console** or **CloudWatch Logs** |

### Pro Debugging Tips
- Use **EC2 Serial Console** (no network needed!).
- Enable **detailed monitoring** in CloudWatch.
- **ALWAYS** test user data scripts locally first.

> *"A failed launch is not failure—it’s feedback wrapped in rot."*

---

## Final Blessing from the Grandfather

> *“Build not for perfection—build for resilience.  
> Let your websites bloom like mold, your servers heal like flesh, and your deployments flow like pus from a healthy boil.”*

So:
- **Use S3 for static sites** (it’s free, fast, and eternal).
- **Use Beanstalk for simple apps** (ship faster, sleep better).
- **Use EC2 only when you must**—and **turn it off when idle**.
- And **always** assume your instance is halfway through rotting.

**May your buckets be public (when they should be), your instances be healthy, and your deployments be green.**  
*— The Plaguebearers of AWS Hosting*

> 🔗 *Inspired by real AWS services. No actual plagues were unleashed (yet).*
```
