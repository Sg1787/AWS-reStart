#  Project: Cloud Security with AWS IAM  
### *“Power without control is chaos unbound.”*

> *“Grant power. Deny betrayal. Bind the unworthy in chains of policy.”*  
> — The Ninth Edict of the Dark Mechanicum

In this ritual of **digital sovereignty**, I forged **granular, unbreakable boundaries** around my AWS realm using **Identity and Access Management (IAM)**. No rogue user. No overprivileged script. Only **precisely measured power**, granted like a **plague-doctor’s dose**—enough to serve, never enough to overthrow.

Behold: a **multi-layered fortress**, built not of stone, but of **JSON sigils**, **user groups**, and **tag-based wards**.

---

##  What is AWS IAM?

**IAM = Identity and Access Management**—the **soul-binding engine** of AWS security.  
It lets you:

- Create **users** (mortal or daemon)  
- Group them into **cohorts of purpose**  
- Grant **exact permissions** via **policies**  
- Tag resources to **separate development from production** like Nurgle separates life from decay

>  *“I didn’t expect how easy it is to create user permissions.”*  
> — Sadiyah Grobbler, after surviving her first IAM ritual

---

##  The Sacred Setup

###  **Account Alias**
- Created a **custom login URL**:  
  ```
  https://nextwork-alias-sadiyahgrobbler.signin.aws.amazon.com/console
  ```
- No more soulless account IDs—only **named thrones** in the Cloud Imperium.
<img width="492" height="317" alt="image" src="https://github.com/user-attachments/assets/84723ff1-a113-4d34-aa59-e04571248906" />


###  **Resource Tagging**
- Tagged EC2 instances with:  
  - **Key**: `Env`  
  - **Values**: `development` | `production`  
- Like **ritual sigils**, these tags let policies **discern friend from foe**, lab from battlefield.

###  **IAM Users & Groups**
- Created a **new IAM user** (a loyal servant)  
- Placed them in a **User Group** (a warband of shared fate)  
- Attached a **custom JSON policy** to the group—so all members **inherit the same divine limits**

---

##  The JSON Policy: A Pact in Code

This is the **heart of the ritual**—a **Tzeentchian contract** written in JSON that:

 **Allows** all EC2 actions…  
 …**but only** on instances tagged with `Env = development` or `Env = production`  
 **Denies** *any* user from **creating or deleting tags** on *any* EC2 instance  

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:*",
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Condition": {
        "StringEquals": {
          "ec2:ResourceTag/Env": ["development", "production"]
        }
      }
    },
    {
      "Effect": "Deny",
      "Action": [
        "ec2:CreateTags",
        "ec2:DeleteTags"
      ],
      "Resource": "arn:aws:ec2:*:*:instance/*"
    }
  ]
}
```

>  **Grandfather’s Wisdom**:  
> *Effect = Allow/Deny* → the voice of judgment  
> *Action = What may be done* → the scope of power  
> *Resource = Where it applies* → the boundary of the realm  

<img width="986" height="263" alt="image" src="https://github.com/user-attachments/assets/0e56ba79-99e4-4a19-944a-c4662f8adfdf" />

---

##  Trial by Fire: Testing the Pact

###  **Production Instance – STOP ATTEMPT**  
- Tried to stop the `production` EC2 instance as the IAM user  
- **DENIED!**  
- Error: _“You are not authorized”_
<img width="1222" height="261" alt="image" src="https://github.com/user-attachments/assets/aed6ffce-b3ac-450a-85bd-ffa0488266c5" />


-  **Success**: The policy **protected the sacred production realm**

### **Development Instance – STOP ATTEMPT**  
- Tried to stop the `development` instance  
- **ALLOWED!**  
- Instance halted cleanly  
-  **Success**: The servant may **tinker in the lab**, but never touch the war engine
<img width="1072" height="271" alt="image" src="https://github.com/user-attachments/assets/a02ce6ea-5fba-47ab-ac5e-b4fbf0c0495b" />

---

##  Key Takeaways (Heresies Learned)

- **Tags are divine filters**—use them to segment reality  
- **Policies should be least-privilege**—grant only what is needed  
- **User Groups > Individual Permissions**—manage warbands, not lone berserkers  
- **Deny statements are absolute**—even if Allow says otherwise  

> *“This whole project took me 1h30m—including documentation.”*  
> — Proof that **security need not be slow**, only **precise**

---

##  Future Defenses (Chaos Upgrades)

- Add **MFA enforcement** for Khornate fury  
- Use **IAM Roles** for Lambda daemons (no passwords, only contracts)  
- Integrate **AWS Organizations + SCPs** to **curse entire accounts** that defy policy

---

> *Documented in blood and JSON by **Sadiyah Grobbler**,*  
> *Warden of the Gate, Binder of Daemons, and Keeper of the Tagged Flame.*  
>   
> 🜂 Thanks to **NextWork** for the sacred project guide!  
>  
> *“Not all who wander are lost… but all who skip IAM are doomed.”*
