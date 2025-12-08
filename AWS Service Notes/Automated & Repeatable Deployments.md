#  Automated & Repeatable Deployments: The Plague Lord’s Guide to Perfect Golems  
## *“A golem built by hand is a golem doomed to rot unevenly.”*  
> — **Grandfather Nurgle**, while deploying 10,000 identical daemons via CloudFormation

Welcome, Architect of the Eternal Swarm!  
In AWS, **consistency is divine**—and **manual deployments are heresy**.  
This guide teaches you to **build, launch, and replicate** your systems like a true Plaguebearer: **automated, idempotent, and gloriously repeatable**.

All hail the **Three Sacred Scrolls**:  
`AMI` • `Launch Templates` • `CloudFormation`

---

##  1. Automated & Repeatable Deployments: Why Bother?

- **Manual = fragile, slow, inconsistent**  
- **Automated = fast, reliable, scalable**  
- **Repeatable = “Works in prod” isn’t a miracle—it’s a guarantee**

>  *"If you can’t rebuild your system in 10 minutes, you don’t own it—you’re just renting chaos."*

---

##  2. AMI Building Strategy: The Perfect Golem Blueprint

An **AMI (Amazon Machine Image)** is a **pre-baked snapshot** of your OS + software + config.

###  Nurgle’s AMI Best Practices
- **Start from AWS-provided AMIs** (Amazon Linux 2023, Ubuntu Pro)
- **Never bake secrets into AMIs** (use SSM Parameter Store or Secrets Manager)
- **Automate AMI creation** with **EC2 Image Builder** or **Packer**
- **Tag AMIs** (`Version: v1.2`, `Env: prod`)

###  Create an AMI from an Instance (CLI)
```bash
aws ec2 create-image \
  --instance-id i-1234567890abcdef0 \
  --name "plague-golem-v1" \
  --description "Pre-hardened, patched, and blessed"
```

>  *"An AMI is a frozen moment of perfection—ready to multiply at will."*

---

##  3. Amazon EC2 Launch Templates: The Golem Summoning Scroll

**Launch Templates** replace **Launch Configurations**—they’re **versioned**, **more flexible**, and **support all modern EC2 features**.

###  What’s Inside a Launch Template?
- AMI ID  
- Instance type  
- Key pair  
- Security groups  
- IAM role  
- User data (startup script)  
- Tags  

###  Create a Launch Template (CLI)
```bash
aws ec2 create-launch-template \
  --launch-template-name plague-golem-template \
  --launch-template-data '{
    "ImageId": "ami-0abcdef1234567890",
    "InstanceType": "t3.micro",
    "KeyName": "my-key",
    "SecurityGroupIds": ["sg-12345678"],
    "IamInstanceProfile": {"Name": "SSM-Instance-Profile"},
    "UserData": "IyEvYmluL2Jhc2gKZWNobyAiVGhlIHJvdCBoYXMgYmVndW4uLi4i",
    "TagSpecifications": [{
      "ResourceType": "instance",
      "Tags": [{"Key": "Owner", "Value": "Nurgle"}, {"Key": "Env", "Value": "prod"}]
    }]
  }'
```

>  *"A Launch Template is a spellbook—each version a refinement of divine will."*

---

##  4. Infrastructure as Code (IaC): The Grand Ritual

**IaC = Define your entire cloud in text files** (JSON/YAML).  
→ Version-controlled.  
→ Peer-reviewed.  
→ **Reproducible anywhere**.

###  Why IaC?
- **No more “works on my machine”**
- **Disaster recovery = `git clone` + deploy**
- **Enforce security & compliance by default**

>  *"Infrastructure without code is sand art—beautiful, temporary, and swept away by the first storm."*

---

##  5. Introduction to JSON and YAML: The Languages of Creation

You’ll write IaC in **JSON** (strict, braces) or **YAML** (clean, indentation-based).

###  JSON Example (CloudFormation snippet)
```json
{
  "Resources": {
    "PlagueBucket": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
        "BucketName": "my-plague-bucket-2025"
      }
    }
  }
}
```

###  YAML Example (Same thing, cleaner)
```yaml
Resources:
  PlagueBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-plague-bucket-2025
```

>  *"YAML is for poets. JSON is for machines. Both serve the Machine God."*

---

##  6. AWS CloudFormation: The Scroll of Rebirth

**CloudFormation** = AWS’s native IaC service.  
You write a **template** → CloudFormation **creates, updates, or deletes** your stack.

###  Core Concepts
- **Template**: YAML/JSON file defining your infra
- **Stack**: Live instance of your template
- **Drift Detection**: “Did someone manually change something?!”

###  Deploy a Stack (CLI)
```bash
# Create stack
aws cloudformation create-stack \
  --stack-name plague-garden \
  --template-body file://plague-garden.yaml \
  --parameters ParameterKey=Env,ParameterValue=prod

# Update stack (auto-detects changes)
aws cloudformation update-stack \
  --stack-name plague-garden \
  --template-body file://plague-garden.yaml
```

>  *"CloudFormation doesn’t just build—it heals. Change the scroll, and the garden reshapes itself."*

---

##  7. Troubleshooting CloudFormation Deployments: When the Ritual Fails

Even the Grandfather’s spells sometimes fizzle.

###  Common Issues & Cures

| Symptom | Likely Cause | Fix |
|--------|-------------|-----|
| **ROLLBACK_IN_PROGRESS** | Resource failed to create (e.g., S3 name taken) | Check **Events tab** in Console → fix template |
| **Circular dependency** | Resource A needs B, B needs A | Use `DependsOn` or restructure |
| **Insufficient permissions** | CloudFormation role missing perms | Attach `AWSCloudFormationFullAccess` (temp) or least-privilege policy |
| **Drift detected** | Someone changed infra manually | **Don’t do that!** Use `aws cloudformation detect-drift` |

###  Debug Like a Plaguebearer
```bash
# See stack events (reverse chronological)
aws cloudformation describe-stack-events --stack-name plague-garden

# Validate template before deploy
aws cloudformation validate-template --template-body file://plague-garden.yaml
```

>  *"A failed stack isn’t failure—it’s feedback wrapped in divine correction."*

---

##  Final Blessing from the Grandfather

> *“Build not by hand—build by scroll.  
> Let every deployment be a copy of perfection, every server a child of the same blessed mold.”*

So:
- **Use AMIs** for consistent base images.
- **Use Launch Templates** for instance config.
- **Use CloudFormation** for everything else.
- **Never click in the Console for prod**.
- And **always** treat your infra like sacred scripture—**version it, review it, and never alter it in secret**.

 **May your deployments be green, your rollbacks be smooth, and your servers be gloriously identical.**  
*— The Plaguebearers of AWS Automation*

>  *Inspired by real AWS services. No actual daemons were summoned (probably).*
