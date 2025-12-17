#  Lab Walkthrough: Monitor an EC2 Instance Like a Chaos Sentinel  
*When Daemons Overclock, the CloudWatch Gods Take Notice*

> *"A silent server is a sleeping daemon. I do not trust silence—I trust thresholds."*  
> — Sadiyah Grobbler, Cloud Sentinel of the Ruinous Powers

In this 60-minute vigil, I didn’t just “monitor” an EC2 instance—I **ensnared it in a web of divine watchfulness**. I built an **SNS alarm** that screams via email when CPU spikes beyond 60%, **simulated a malware-driven possession** by overloading the CPU to 100%, and **watched the alarm shriek into my inbox** like a warp-beacon warning of invasion.

Then, I forged a **CloudWatch dashboard**—a single pane of glass through which I survey the health of my digital warhost.

This is **observability as holy defense**.

---

##  Step 1: Forge the SNS Herald (The Crying Prophet)

Before any alarm can sound, it needs a **voice that reaches the mortal realm—inbox**.

1. Opened **Simple Notification Service (SNS)**
2. Created a **Standard topic** named `MyCwAarm` (yes, I typo’d it once—Khorne laughed)
3. Subscribed my **email address** via **Email protocol**
4. **Confirmed the subscription** by clicking the link in the AWS confirmation email
<img width="678" height="257" alt="image" src="https://github.com/user-attachments/assets/f02fb3b4-8fca-4bb4-8e5b-ed5e40901d48" />

>  *Until confirmed, the topic was mute—a prophet with a sealed mouth. Once confirmed, it could scream across dimensions.*

Now, any alarm tied to this topic would **summon a message directly to my inbox**—no delays, no intermediaries.

---

##  Step 2: Conjure the CloudWatch Alarm (The All-Seeing Eye)

Next, I bound the **Stress Test EC2 instance** to a **watchful spirit** that triggers when CPU crosses 60%.

1. Went to **CloudWatch > Metrics > EC2 > Per-Instance Metrics**
2. Found the **Stress Test** instance and selected its **CPUUtilization** metric
3. Clicked **Create alarm** with these bindings:
   - **Metric**: `CPUUtilization`
   - **Statistic**: `Average`
   - **Period**: `1 minute`
   - **Condition**: **Greater than 60%**
   - **Alarm name**: `LabCPUUtilizationAlarm`
   - **Action**: **Send notification to `MyCwAlarm` SNS topic when "In alarm"**
<img width="1273" height="193" alt="image" src="https://github.com/user-attachments/assets/dd55e5fd-987a-4f26-a748-6735118318d0" />

>  *This alarm doesn’t guess—it watches. Every 60 seconds, it samples the CPU. Cross 60%, and the Warp trembles.*

I clicked **Create alarm**, and the Eye opened.

---

##  Step 3: Summon the Daemon (Stress Test = Simulated Possession)

Time to **invite chaos**—and see if my defenses hold.

1. From the **Vocareum AWS Details**, I copied the **EC2InstanceURL** (an SSM Session Manager link)
2. Pasted it into a new tab → **connected directly to the EC2 instance terminal**
3. Ran the **possession command**:
   ```bash
   sudo stress --cpu 10 -v --timeout 400s
   ```
   → This spawns 10 worker threads, **maxing CPU to 100%** for **400 seconds** (6+ minutes)—long enough to trigger the alarm.

4. In a **second terminal**, I ran:
   ```bash
   top
   ```
   → Watched **%CPU spike to 100%**—a clear sign of **unnatural activity** (i.e., malware or overenthusiastic daemons)

5. Switched back to **CloudWatch Alarms**, refreshed every 60 seconds...

    **Minute 1**: Alarm = **OK**  
    **Minute 2**: Alarm = **ALARM**   
   → The graph **surged past 60%**, and the status flipped.

6. Checked my **email**—within 90 seconds, I received:
   > **Subject**: `ALARM: "LabCPUUtilizationAlarm" in US-EAST-1`
   >  
   > *"The CPUUtilization metric is above the threshold (60)..."*

>  *The alarm worked. The daemon was seen. The sentinel did not sleep.*

>  **Struggle**: My first attempt used `--timeout 60s`—too short! The alarm needs **two consecutive periods** above threshold (by default). **400s ensured sustained breach**. Lesson: *Malware doesn’t blink—it burns.*

---

##  Step 4: Create the CloudWatch Dashboard (The War Room)

Why stare at raw metrics when you can build a **tactical command center**?

1. In **CloudWatch > Dashboards > Create dashboard**
2. Named it: `LabEC2Dashboard`
3. Added a **Line widget**
4. Selected:
   - **Namespace**: `AWS/EC2`
   - **Metric**: `CPUUtilization`
   - **Instance**: `Stress Test`
5. Clicked **Create widget > Save dashboard**

Now, with one click, I see the **pulse of my instance**—a live EKG of its sanity (or corruption).

>  *Slaanesh would admire this: elegant, informative, and deeply aesthetic.*
<img width="577" height="455" alt="image" src="https://github.com/user-attachments/assets/2c0fcc21-de7f-489b-8f9e-127facafc150" />
---

##  Lab Complete: A Sentinel’s Oath Fulfilled

I have now:
-  **Created an SNS topic** and **confirmed email subscription**
-  **Configured a CloudWatch alarm** tied to CPU > 60%
-  **Simulated a malware-style CPU spike** using `stress`
-  **Received real-time email alert** upon breach
-  **Built a CloudWatch dashboard** for ongoing vigilance

This lab taught me: **Monitoring isn’t passive—it’s prophylactic warfare**.  
In the cloud, **you don’t wait for breaches—you detect them before they bloom**.

> *"The best security engineers don’t fight fires. They build smoke detectors that scream before the first spark."*

— Sadiyah Grobbler  
**AWS Re/Start Acolyte | Aspiring Security Engineer | Keeper of the All-Seeing CloudWatch Eye**
