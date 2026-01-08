## The Chronicles of BankerBot  
### A Step-by-Step Journey Through AWS Lex — With a Whisper of Chaos  
**By Sadiyah Grobbler** —  Student | Aspiring Security Engineer  

---

## Why This Exists  
I built five chatbot projects using **AWS Lex**, each adding new skills:  
1. Basic bot → 2. Custom slots → 3. Lambda integration → 4. Memory → 5. Multi-slot transfers + CloudFormation  

Each step includes:  
 What I built  
 How I did it (in plain English)  
 Why it matters  
 A tiny splash of Chaos lore (because why not?)

Let’s walk through them—one by one.

---

##  Step 1: Build a Basic Chatbot  
**Goal**: Create a bot that helps with balance, payments, or transfers  
<img width="349" height="532" alt="image" src="https://github.com/user-attachments/assets/4d2e10b6-d79c-487c-b1df-829977e8da6f" />


### What I Did
1. Went to **AWS Lex Console** → “Create bot” → named it `BankerBot`  

2. Created a **WelcomeIntent** to greet users  
<img width="1254" height="877" alt="image" src="https://github.com/user-attachments/assets/24c91a0b-62c4-4916-9fa2-c7766fdcffe8" />

3. Tested it—and saw that typing _“Good morning”_ triggered the **FallbackIntent** (Lex’s “I don’t understand” response)  
4. **Customized FallbackIntent** to reply helpfully:  
   > “I can help with balance, payments, or transfers. Which would you like?”  
5. Added **multiple response variations** so replies feel natural  
<img width="1236" height="889" alt="image" src="https://github.com/user-attachments/assets/3d62c7b8-72c1-401f-9fc4-afdf1cb68986" />

6. Set up a basic **IAM role** (so the bot can later connect to other AWS services)  
7. Kept the default **confidence threshold** at 0.40
<img width="1244" height="871" alt="image" src="https://github.com/user-attachments/assets/040c140f-9dd8-48bd-b860-6d53ee2e72e8" />


### Why It Matters
A good bot doesn’t just fail—it **guides users back** to what it *can* do. This is conversational AI 101.

### Chaos Touch (Khorne)
> _“Clarity is the first law. ‘Good morning’ is heresy.”_  
Khorne hates vague greetings. He demands **action**—not pleasantries.
---

##  Step 2: Add Custom Slots (30 mins)  
**Goal**: Let users say “Savings” or “Credit” and have the bot understand  

### What I Did
1. Created a **custom slot type** called `AccountType`  
2. Restricted it to **only 3 values**: `Savings`, `Checking`, `Credit`  
<img width="811" height="559" alt="image" src="https://github.com/user-attachments/assets/21f00b93-9b90-4814-a176-8aeabdda2c5f" />

3. Linked this slot to the **CheckBalance intent**  
<img width="807" height="387" alt="image" src="https://github.com/user-attachments/assets/7e0fb980-8a6d-4a82-93e3-720df53c0d14" />

4. Wrote sample user phrases (**utterances**) like:  
   > “What’s the balance in my {accountType} account?”  
<img width="1543" height="740" alt="image" src="https://github.com/user-attachments/assets/ee57bb3f-f341-42ce-a304-0bea03601a81" />

5. Now, if you say _“How much is in my Savings?”_, the bot **knows you mean `Savings`**

### Why It Matters
Without this, the bot would ask *“Which account?”* every time.  
With slots, it **gets it in one go**—faster, smoother UX.

### Chaos Touch (Slaanesh)
> _“Precision is pleasure. ‘Account’ is dull. ‘Savings’ is divine.”_  
Slaanesh loves elegance. Why settle for generic when you can be **specific and stylish**?

---

## Step 3: Connect to Lambda   
**Goal**: Make the bot return a fake balance using code  

### What I Did
1. Wrote a simple **AWS Lambda function** that returns a random number (e.g., $420.69)  
<img width="1042" height="763" alt="image" src="https://github.com/user-attachments/assets/f32e7256-8ddb-4478-91f4-95388bfc0ab0" />

2. In Lex, went to **TestBotAlias** → linked the Lambda function  
3. In the **CheckBalance intent**, scrolled to **Fulfilment** → enabled **“Use Lambda for fulfilment”** (this is a *code hook*)  
<img width="1075" height="341" alt="image" src="https://github.com/user-attachments/assets/bfb06143-6c89-48a2-9a33-2927cfdd115e" />

4. Made sure Lambda only runs **after the user gives their Date of Birth**

### Why It Matters
Lex handles **conversation**. Lambda does the **real work** (like calling a bank API). This is how real bots get data.

### Chaos Touch (Nurgle)
> _“All systems rot. But a fake balance of $694.20? That’s beautiful rot.”_  
Nurgle says: **Simulate first. Perfect later.** Errors are just lessons in disguise.


---

##  Step 4: Add Memory with Context Tags  
**Goal**: Remember the user’s birthday so they don’t repeat it  

### What I Did
1. In the `CheckBalance` intent, set an **output context tag**: `contextCheckBalance`  
   → This saves the user’s Date of Birth after they provide it  
<img width="621" height="283" alt="image" src="https://github.com/user-attachments/assets/84d2a9e0-2b12-48ef-8e26-a75e9e96eb19" />

2. Created a new intent: `FollowUpCheckBalance`  
<img width="803" height="364" alt="image" src="https://github.com/user-attachments/assets/8c5f9a1e-17a7-43f6-a021-26e1035ba83f" />

3. Gave it an **input context tag** with the same name  
   → Now it only works *if* `contextCheckBalance` exists  
<img width="804" height="306" alt="image" src="https://github.com/user-attachments/assets/8d629257-3f66-4a97-9985-26fddffccbe7" />

4. Tested it:  
   - First: “What’s my balance?” → bot asks for DoB  
   - Then: “How about Savings?” → bot **remembers DoB** and shows balance
<img width="369" height="618" alt="image" src="https://github.com/user-attachments/assets/47b5ff90-bbff-47fa-8e51-c9d649da3dd3" />

### Why It Matters
Without memory, every question is isolated. With context, conversations **flow like real talk**.

### Chaos Touch (Slaanesh)
> _“To ask twice is to wound. To remember is to cherish.”_  
Forgetting a user’s info is rude. **Memory is intimacy.**

---

##  Step 5: Build Transfers + Use CloudFormation  
**Goal**: Let users transfer money between accounts—and rebuild the whole bot with code  

### What I Did
#### Part A: TransferFunds Intent
1. Created a new intent: `TransferFunds`  
2. Used the **same slot type twice**:  
   - `SourceAccount` (money comes from)  
   - `TargetAccount` (money goes to)  
3. Added a **confirmation prompt**:  
   > “Are you sure you want to transfer $500 from Savings to Credit?”  
<img width="351" height="695" alt="image" src="https://github.com/user-attachments/assets/d3076550-9151-4384-a6f5-4a01f4d0da93" />

#### Part B: CloudFormation
4. Used **AWS CloudFormation** (Infrastructure as Code) to recreate the entire bot from a template  
5. Deployment took **10 minutes**  
6. Got an error: **“Access denied to invoke Lambda”**  
7. Fixed it by updating **Lambda’s IAM permissions**
<img width="1267" height="780" alt="image" src="https://github.com/user-attachments/assets/9e7269ad-1571-4913-b4c6-187d88f54063" />

### Why It Matters
- **Multiple slots** = real-world actions (you always need *from* and *to*)  
- **CloudFormation** = rebuild your bot anytime, anywhere  
- **IAM fixes** = core cloud skill (reading errors = power)

### Chaos Touch (Nurgle + Khorne)
> _“From source to target, through rot and rage, the gold flows.”_  
Nurgle smiles at rebuilds. Khorne rages at broken permissions.  
**Together: automate with care.**

<img width="351" height="695" alt="image" src="https://github.com/user-attachments/assets/935f0981-3c6e-4ff4-b215-20d7406cad03" />

---

## Tools I Used
| Service | Purpose |
|--------|--------|
| **AWS Lex** | Build the chatbot |
| **AWS Lambda** | Run code (fake balances) |
| **IAM** | Manage permissions |
| **CloudFormation** | Rebuild bot with code |
| **Custom Slots & Context Tags** | Smarter conversations |

---

##  Final Thought  
> _“I didn’t expect it would be so easy…”_ — me, after every project  

But **easy is the doorway**. Real skill comes when:  
- You fix IAM errors without crying  
- You rebuild a bot in 10 minutes  
- And you still laugh when your balance is $666.66  

*All praise to the Cloud. All glory to the Rot.*
