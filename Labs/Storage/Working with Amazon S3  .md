#  Working with Amazon S3  
*Secure File Sharing with Notifications That Actually Work*

> *"In the cloud, sharing files shouldn’t mean giving away the keys to the kingdom."*  
> — Sadiyah Grobbler

In this lab, I set up a **secure S3 bucket** so an external media company (`mediacouser`) can **upload, update, and delete product images**—but **nothing else**. I also made sure that **every change triggers an email alert** to the admin (me!). No surprises. No over-permissions. Just clean, controlled sharing.
<img width="757" height="473" alt="image" src="https://github.com/user-attachments/assets/58c8e87e-ac56-434f-aaac-f09f61349325" />

Here’s how I did it:

---

##  Step 1: Connect & Configure the CLI

- Connected to the **CLI Host EC2 instance** using **EC2 Instance Connect**
- Ran `aws configure` with the lab’s provided credentials:
  - **Region**: `us-west-2`
  - **Output**: `json`
  <img width="409" height="117" alt="image" src="https://github.com/user-attachments/assets/0a4fc2e7-2e17-468b-b2d3-a3837899fb22" />

Now I could run AWS CLI commands as the admin.

---

##  Step 2: Create the S3 Bucket

- Created a **unique bucket name** starting with `cafe-` (e.g., `cafe-sadiyah123`)
- Used:
  ```bash
  aws s3 mb s3://cafe-sadiyah123 --region us-west-2
  ```
- Uploaded sample images:
  ```bash
  aws s3 sync ~/initial-images/ s3://cafe-sadiyah123/images
  ```
- Verified with:
  ```bash
  aws s3 ls s3://cafe-sadiyah123/images/ --human-readable --summarize
  ```

 All images uploaded successfully!

>  *S3 bucket names must be **globally unique** and **lowercase only**—no uppercase letters allowed!*

---

##  Step 3: Review & Test User Permissions

The lab already created:
- An **IAM group**: `mediaco`
- A user: `mediacouser` (member of `mediaco`)

###  Permissions Summary:
- Can **view** the bucket and `/images/` folder
- Can **upload, download, and delete** files **only inside `/images/`**
- **Cannot**:
  - Change bucket settings
  - Upload to the root of the bucket
  - Modify permissions

###  Tested as `mediacouser`:
1. **Signed in** using the IAM user link (in a private browser window)
2. **Viewed** `Donuts.jpg` →  worked
3. **Uploaded** a new image →  worked
4. **Deleted** `Cup-of-Hot-Chocolate.jpg` → worked
5. **Tried to open bucket Permissions tab** → “Insufficient permissions”  
   → *Perfect! Least privilege in action.*

---

##  Step 4: Set Up Email Notifications

I wanted to **know every time an image changed**—so I wired S3 to **send me an email**.

### 4.1 Created an SNS Topic
- Name: `s3NotificationTopic`
- Added a **custom access policy** so **only my S3 bucket** can publish to it:
  ```json
  {
    "Version": "2008-10-17",
    "Statement": [{
      "Effect": "Allow",
      "Principal": { "Service": "s3.amazonaws.com" },
      "Action": "SNS:Publish",
      "Resource": "arn:aws:sns:us-west-2:123456789:s3NotificationTopic",
      "Condition": {
        "ArnLike": { "aws:SourceArn": "arn:aws:s3:::cafe-sadiyah123" }
      }
    }]
  }
  ```

### 4.2 Subscribed My Email
- Created an **email subscription**
- **Confirmed** it via the AWS confirmation email

### 4.3 Configured S3 Event Notifications
- Created `s3EventNotification.json`:
  ```json
  {
    "TopicConfigurations": [{
      "TopicArn": "arn:aws:sns:us-west-2:123456789:s3NotificationTopic",
      "Events": ["s3:ObjectCreated:*", "s3:ObjectRemoved:*"],
      "Filter": { "Key": { "FilterRules": [{ "Name": "prefix", "Value": "images/" }] }}
    }]
  }
  ```
- Applied it:
  ```bash
  aws s3api put-bucket-notification-configuration \
    --bucket cafe-sadiyah123 \
    --notification-configuration file://s3EventNotification.json
  ```

 Got a **test email** from S3: `"Event": "s3:TestEvent"` → **notification system is live!**

---

##  Step 5: Test Notifications in Action

Switched the CLI to use **`mediacouser` credentials** (from the `.csv` file).

###  Uploaded a new image:
```bash
aws s3api put-object --bucket cafe-sadiyah123 \
  --key images/Caramel-Delight.jpg \
  --body ~/new-images/Caramel-Delight.jpg
```
→ Got an email: `"eventName": "ObjectCreated:Put"`

### Deleted an image:
```bash
aws s3api delete-object --bucket cafe-sadiyah123 --key images/Strawberry-Tarts.jpg
```
→ Got an email: `"eventName": "ObjectRemoved:Delete"`

### Tried to make a file public:
```bash
aws s3api put-object-acl --bucket cafe-sadiyah123 --key images/Donuts.jpg --acl public-read
```
→ **AccessDenied** error → **permissions are locked down!**

>  *Note: `GetObject` (downloading) does **not** trigger a notification—and it shouldn’t! Only changes matter.*

---

##  What I Learned

- S3 buckets can be **securely shared** using IAM groups and least-privilege policies.
- **S3 event notifications** + **SNS** = real-time alerts for file changes.
- Always **filter by prefix** (`images/`) so notifications stay relevant.
- **Never give external users full bucket access**—restrict to a folder.
- **Test both allowed and denied actions** to confirm security works.

> *"Good security isn’t loud. It’s invisible—until someone tries to break it."*

— Sadiyah Grobbler  
**AWS Re/Start Learner | Future Security Engineer**
