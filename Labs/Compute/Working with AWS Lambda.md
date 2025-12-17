#  Lab Walkthrough: Working with AWS Lambda  
*Where Serverless Meets Sacrifice—and a Missing Inbound Rule Nearly Doomed Us All*

> *"I do not summon daemons lightly. But a Lambda function that fails to connect to a database? That’s a daemon I *will* slay."*  
> — Sadiyah Grobbler, Chaos Lambda Engineer

This was **the most maddening, intricate, and rewarding lab** I’ve faced. A **two-function serverless pipeline**, triggered by time, extracting data from a hidden database, and emailing a sacred sales report—all while dancing through IAM roles, VPC attachments, and the silent wrath of **port 3306**.

And yes—I **almost broke**. But in the end? The report flew. The email arrived. The Warp was held at bay.

---

##  The Architecture: A Rite in Six Steps

The design was elegant—if unforgiving:
1. **CloudWatch Event** fires daily at 8 PM (Mon–Sat)
2. **`salesAnalysisReport`** Lambda wakes up
3. It **invokes** `salesAnalysisReportDataExtractor`
4. The extractor **pulls sales data** from a MySQL DB on an EC2 LAMP stack
5. Results return; main function **formats a report**
6. **SNS emails it** to the admin

<img width="773" height="415" alt="image" src="https://github.com/user-attachments/assets/31b4cfe8-3ef6-480c-a938-59d571571da0" />

Beautiful. In theory.

In practice? **Blood, logs, and one missing security rule.**

---

##  Step 1: IAM Roles – The Soul Contracts

Before any Lambda speaks, it must wear a **sigil of permission**.

###  `salesAnalysisReportRole` (Main Function)
- **Trusts**: `lambda.amazonaws.com`
- **Policies**:
  - `AmazonSNSFullAccess` → *to scream via email*
  - `AmazonSSMReadOnlyAccess` → *to read DB secrets from Parameter Store*
  - `AWSLambdaBasicRunRole` → *to write logs*
  - `AWSLambdaRole` → *to invoke other Lambdas*

###  `salesAnalysisReportDERole` (Data Extractor)
- **Trusts**: `lambda.amazonaws.com`
- **Policies**:
  - `AWSLambdaBasicRunRole`
  - `AWSLambdaVPCAccessRunRole` → *critical for VPC access*

>  *Without `VPCAccessRunRole`, the function can’t spawn ENIs—and dies silently. Tzeentch loves tricks like that.*

---

##  Step 2: Layers & Code – Binding External Souls

###  Created `pymysqlLibrary` Layer
- Uploaded `pymysql-v3.zip`
- Set runtime: **Python 3.9**
- Attached it to the extractor function

>  **Why?** Lambda runtimes don’t include `pymysql` by default. The layer **injects the library** like a daemon’s blessing.

###  Built `salesAnalysisReportDataExtractor`
- **Runtime**: Python 3.9
- **Role**: `salesAnalysisReportDERole`
- **Handler**: `salesAnalysisReportDataExtractor.lambda_handler`
- **Code**: Uploaded `salesAnalysisReportDataExtractor-v3.zip`
- **VPC Config**:
  - **VPC**: `Cafe VPC`
  - **Subnet**: `Cafe Public Subnet 1`
  - **Security Group**: `CafeSecurityGroup`
<img width="1336" height="498" alt="image" src="https://github.com/user-attachments/assets/3590c612-6a0f-4cac-b846-4cba62af56e1" />

>  **Here’s where hell opened**: I *thought* the security group allowed port 3306.  
> But the **EC2 instance’s SG** and the **Lambda’s VPC config** must **both** permit it—and **in both directions**.

---

##  Step 3: The Great Failure – And How I Slew It

I ran the test with:
```json
{
  "dbUrl": "...",
  "dbName": "cafe_db",
  "dbUser": "...",
  "dbPassword": "..."
}
```

**Result**:  `Execution result: failed`

No useful error. Just silence.  
I checked:
- IAM?   
- Layer?  
- VPC subnet?   
- **Security group inbound rules?** 

###  The Revelation
I went to **EC2 > Security Groups > CafeSecurityGroup**  
→ **No inbound rule for port 3306 from the Lambda’s security group (or VPC CIDR)!**

**Fix**: Added inbound rule:
- **Type**: MySQL (3306)
- **Source**: `10.0.0.0/16` (the Cafe VPC CIDR)

>  *The Lambda lives in the VPC. The DB must accept traffic from the VPC—not just “anywhere.” This is zero-trust networking in action.*

Ran the test again.  
 **“Execution result: succeeded (logs)”**  
I nearly wept.

---

##  Step 4: SNS – The Herald of Reports
<img width="1366" height="432" alt="image" src="https://github.com/user-attachments/assets/44f7318d-554d-4a13-8f0f-a10e4ac5c5c8" />

1. Created **SNS topic**: `salesAnalysisReportTopic`
2. Subscribed my **email**
3. **Confirmed** via AWS confirmation email
<img width="780" height="288" alt="image" src="https://github.com/user-attachments/assets/8c934c57-fd39-437a-b0df-bb6fea80425f" />
<img width="791" height="264" alt="image" src="https://github.com/user-attachments/assets/be5c49e8-7e14-4a8e-82a1-7107cfbf07f8" />

>  *Slaanesh demands elegance—even in notifications. A clean subject line, a timely delivery. This was art.*

---

##  Step 5: Main Lambda – The Conductor

Built `salesAnalysisReport` **via AWS CLI** on the **CLI Host EC2**:
```bash
aws lambda create-function \
  --function-name salesAnalysisReport \
  --runtime python3.9 \
  --handler salesAnalysisReport.lambda_handler \
  --role arn:aws:iam::...:role/salesAnalysisReportRole \
  --zip-file fileb://salesAnalysisReport-v2.zip \
  --region us-west-2
```

Then in console:
- Added **env var**: `topicARN = [SNS ARN]`
- Tested →  success
- **Added CloudWatch Event trigger**:
  - **Rule**: `salesAnalysisReportDailyTrigger`
  - **Schedule**: `cron(35 11 ? * MON-SAT *)` → *8:35 PM local (UTC-3)*

>  *Cron expressions are Tzeentch’s riddles. Double-check your timezones.*
<img width="412" height="54" alt="image" src="https://github.com/user-attachments/assets/ce618728-66be-405d-bd5a-6065f8024153" />

---

##  Step 6: Feed the Beast – Place Orders!

To see real data:
1. Visited `http://[CafePublicIP]/cafe`
2. Placed **3 fake orders** (coffee, croissant, chaos)
3. Re-ran the extractor → **report now included real sales!**

---

##  Lab Complete: A Serverless Rite Perfected

I now understand:
- **IAM roles are non-negotiable**—each permission must be explicit
- **Lambda + VPC = ENI + SG rules**—or silent failure
- **Layers solve dependency hell**
- **CloudWatch Events = cron for gods**
- **SNS = your messenger to mortals**
- **Logs are your only truth** when things burn

> *"Serverless isn’t magic. It’s precision. One missing slash in a policy, one closed port—and the ritual fails."*

— Sadiyah Grobbler  
**AWS Re/Start Acolyte | Aspiring Security Engineer | Slayer of Silent Lambda Failures**

---

> 💡 **Key Takeaways**:
> - Always **validate VPC security groups** for Lambda-to-DB traffic
> - Use **CloudWatch Logs** religiously (`/aws/lambda/function-name`)
> - **Test incrementally**: extractor first, then main function
> - **Never assume**—even “preconfigured” labs have hidden gaps

---

This lab broke me. Then it rebuilt me stronger.  
Now, I fear no Lambda.

Drop this into your repo—and may your functions always return `succeeded`. 🔥
