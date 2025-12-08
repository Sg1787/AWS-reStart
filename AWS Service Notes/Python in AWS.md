#  Simple Python & AWS DevOps Cheat Sheet  
## *“Code rots. Automate it anyway.”*  
> — **Grandfather Nurgle**, while deploying a Lambda function from a mold-covered terminal

Welcome, script-weaver of the cloud!  
This guide blends **real Python fundamentals** with **AWS automation magic**—all through the eyes of **Nurgle**, who knows that even the cleanest code will eventually decay… so you’d better automate its rebirth.

---

##  I. Core Python: The Language of Rotten Logic

Python is your **plague-bearer’s tongue**—simple, readable, and powerful.

| Concept | Nurgle’s Translation | Code (Keep It Slimy) |
|--------|----------------------|----------------------|
| **Hello, World** | *"Announce your presence to the void."* | `print("Hello, festering world!")` |
| **Data Types** | Numbers for counting boils, text for naming them. | `boils = 42`<br>`plague_name = "Green Pox"` |
| **Lists, Tuples, Dicts** | - **List**: A sack of squirming maggots (you can add/remove).<br>- **Tuple**: A sealed jar of relics (fixed forever).<br>- **Dict**: A ledger of sins (`"instance_id": "i-12345"`). | `tags = {"env": "prod", "owner": "nurgle"}` |
| **Flow Control** | Make decisions like a true Plague Lord.<br>Loop until the rot is complete. | `for instance in instances: stop(instance)` |
| **Functions** | A **reusable curse**. Write once, cast many times. | `def summon_backup(): ...` |
| **Caesar Cipher** | A simple spell to hide secrets (or just impress mortals). | `def encrypt(text, shift): ...` |

>  *"A script without functions is just a list of commands waiting to fester."*

---

##  II. System Tools: Summoning External Powers

Your Python script doesn’t live in isolation—it **calls upon libraries** like a shaman calling spirits.

| Tool | Purpose | Code |
|------|--------|------|
| **Modules (`import`)** | Borrow power from others. Need AWS? `import boto3`. | `import boto3` |
| **File Handlers** | Read config scrolls or write logs to the Book of Rot. | python with open('secrets.json') as `f:cfg = json.load(f)` |
| **System Admin Tasks** | Command the OS directly—clean logs, kill processes, etc. | `import os; os.system('rm -rf /tmp/rot/*')` |

>  *"Even Grandfather Nurgle needs to read the config file."*

---

## III. Debugging & Testing: Tending the Rot

All code decays. Your job? **Find the mold before it spreads.**

| Practice | Nurgle’s Advice | Code/Command |
|--------|------------------|-------------|
| **Debugger (`pdb`)** | Pause time. Inspect your rotting variables. | `import pdb; pdb.set_trace()` |
| **Debugging Flow** | Use `n` (next), `p var` (print variable) to trace the infection. | `(Pdb) p instance_id` |
| **Unit Tests + Mocking** | Test your spells **without summoning real daemons**.<br>→ **Mock AWS calls** so you don’t accidentally delete prod! | `python @patch('boto3.client') def test_stop_instance(mock_client):...` |

>  *"A script untested is a plague unleashed."*

---

##  IV. AWS Automation: The Grand Ritual

With **Boto3**, you command AWS like a true daemon prince.

| Concept | Nurgle’s Blessing | Boto3 Example |
|--------|-------------------|---------------|
| **Automation vs Orchestration** | - **Automation**: One spell (e.g., “stop this instance”).<br>- **Orchestration**: A full ritual (CodePipeline: build → test → deploy). | *Conceptual* |
| **AWS Lambda** | *"Code that lives in the aether—no server, no upkeep, just pure function."* | python `lambda_client = boto3.client('lambda') lambda_client.invoke(FunctionName='clean_rot')` |
| **CI/CD (CodePipeline)** | Automate your entire release cycle—so mortals don’t click buttons. | *Scripts triggered by pipeline stages* |
| **Infrastructure as Code (IaC)** | Define your empire in **text** (CloudFormation/Terraform).<br>If it burns down? Rebuild with one command. | python `cfn = boto3.client('cloudformation') cfn.create_stack(StackName='PlagueGarden', TemplateURL='s3://...')` |
| **Config Management** | Enforce order in chaos: tag all instances, lock down S3, patch EC2. | python ` ec2 = boto3.client('ec2') ec2.stop_instances(InstanceIds=['i-rotten123'])` |

>  *"Infrastructure without code is a sandcastle—beautiful, but doomed to wash away."*

---

## Final Wisdom from the Grandfather

> *“Write scripts that expect to fail. Test them like they’re already broken. And automate their rebirth.”*

So:
- **Use functions**—don’t repeat yourself.
- **Mock AWS calls**—don’t nuke prod.
- **Automate everything**—even your coffee.
- **Embrace decay**—then script your way out of it.

**Happy scripting, Plaguebearer! May your functions be pure, your mocks be faithful, and your deployments be green.** 
