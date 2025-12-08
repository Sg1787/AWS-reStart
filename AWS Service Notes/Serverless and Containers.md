# AWS Serverless & Containers: The Plague Lord’s Guide to Stateless Rot  
## *“Why grow a golem… when you can summon a spirit?”*  
> — **Grandfather Nurgle**, while deploying a Lambda function from a mold-covered Raspberry Pi

Welcome, Weaver of Ephemeral Magic!  
In the grand garden of AWS, you no longer need **servers**—just **intent**.  
This guide covers **serverless spells**, **API rituals**, and **container swarms**—all designed to **rot gracefully, scale infinitely, and vanish when done**.

All hail the **Three Paths of Modern Decay**:  
`Lambda` • `API Gateway` • `Containers`

---

##  1. Serverless & Containers Overview: Flesh vs. Spirit vs. Swarm

| Path | What It Is | When to Use | Nurgle’s Verdict |
|------|-----------|------------|------------------|
| **AWS Lambda** | Run code **without servers**. Pay per millisecond. | Event-driven tasks (e.g., resize images, process logs). | *“Spirits that act, then dissolve into mist.”* |
| **Containers (ECS/EKS)** | Package apps into portable “jars of rot.” | Microservices, legacy app modernization. | *“Swarm-bearers in enchanted jars.”* |
| **Fargate** | Run containers **without managing servers**. | “I want containers, but not the golems to run them.” | *“Pure container essence—no OS, no fuss.”* |

>  *"The best server is the one you never had to log into."*

---

##  2. AWS Lambda: The Spirit of Automation

**Lambda** runs your code in response to **events**—and vanishes when done. No servers. No OS. No patching.

###  Core Concepts
- **Function**: Your code + config (runtime, memory, timeout).
- **Trigger**: What wakes the spirit (S3 upload, API call, CloudWatch event).
- **Concurrency**: How many spirits can run at once (auto-scales up to 1,000+).

### Simple Lambda (Python Example)
```python
# lambda_function.py
def lambda_handler(event, context):
    print("The rot has begun...")
    return {
        'statusCode': 200,
        'body': 'Hello from the Plague Lord!'
    }
```

###  Deploy via CLI
```bash
# Zip your code
zip function.zip lambda_function.py

# Create the function
aws lambda create-function \
  --function-name plague-greeter \
  --runtime python3.12 \
  --role arn:aws:iam::123456789012:role/lambda-execution-role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip
```

>  *"A Lambda function is like a sneeze—sudden, powerful, and gone before you know it."*

---

##  3. APIs and REST: The Language of Spirits

**REST APIs** let your apps talk to the world using simple **HTTP verbs**:
- `GET` = “Show me the rot.”
- `POST` = “Add new rot.”
- `PUT` = “Replace this rot.”
- `DELETE` = “Remove this rot.”

> *"An API without documentation is a curse whispered in the dark."*

---

##  4. Amazon API Gateway: The Spirit Gate

**API Gateway** is your **divine portal**—exposing Lambda, EC2, or HTTP endpoints as clean, secure APIs.

###  Key Features
- **REST/HTTP APIs**: Lightweight, fast, cheap.
- **WebSocket APIs**: Real-time (e.g., chat, live dashboards).
- **Authentication**: IAM, Cognito, or Lambda authorizers.
- **Throttling & Caching**: Protect your backend from greedy mortals.

###  Create a REST API (CLI)
```bash
# Create API
aws apigateway create-rest-api --name "Plague-API"

# Deploy it
aws apigateway create-deployment \
  --rest-api-id YOUR_API_ID \
  --stage-name prod
```

>  *"API Gateway is the bridge between mortal requests and spirit logic."*

---

##  5. AWS Step Functions: The Grand Ritual

When one Lambda isn’t enough—**chain them** into a **visual workflow**.

### Use Cases
- Order processing: `Validate → Charge → Ship → Notify`
- Data pipelines: `Extract → Transform → Load`
- Error handling: Retry, catch, escalate

###  Visual Workflow (ASL - Amazon States Language)
```json
{
  "Comment": "Plague Spreading Workflow",
  "StartAt": "InfectVillage",
  "States": {
    "InfectVillage": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:infect",
      "Next": "SpreadToNeighboringTown"
    },
    "SpreadToNeighboringTown": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-east-1:123456789012:function:spread",
      "End": true
    }
  }
}
```

>  *"Step Functions turn chaos into ceremony."*

---

##  6. Containers on AWS: The Swarm in a Jar

When you need **more control than Lambda**, but **less hassle than EC2**—use **containers**.

### AWS Container Services

| Service | What It Is | Nurgle’s Take |
|--------|-----------|--------------|
| **Amazon ECS** | AWS’s native container orchestrator. Simple, integrated. | *“Swarm management for mortal minds.”* |
| **Amazon EKS** | Managed Kubernetes. For the ritual-purists. | *“The ancient tongue of swarm-herding.”* |
| **AWS Fargate** | Run containers **without managing servers**. | *“Pure container essence—no golems required.”* |

### Deploy a Container (ECS + Fargate)
```bash
# Push image to ECR (Elastic Container Registry)
aws ecr get-login-password | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com
docker build -t plague-app .
docker tag plague-app:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/plague-app:latest
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/plague-app:latest

# Then deploy via ECS task definition + service
```

>  *"A container is a golem in a bottle—portable, pure, and ready to bloom."*

---

##  Final Blessing from the Grandfather

> *“Build not with stone, but with spirit.  
> Let your code rise on demand, vanish when idle, and scale like a plague across the land.”*

So:
- **Use Lambda** for event-driven tasks.
- **Use API Gateway** to expose them cleanly.
- **Use Step Functions** for complex rituals.
- **Use Fargate** when you need containers—but not the burden.

**May your functions be stateless, your APIs be RESTful, and your containers never leak.**  
*— The Plaguebearers of AWS Serverless*

> 🔗 *Inspired by real AWS services. No actual plagues were unleashed (yet).*
```
