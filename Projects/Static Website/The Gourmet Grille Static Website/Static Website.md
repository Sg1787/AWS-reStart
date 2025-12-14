#  Project 1: The Gourmet Grille – Static Website on AWS  
> _“From paper chaos to cloud clarity—one S3 bucket at a time.”_

##  Team Roles, Shared Mission  
This project was a group effort during the **AWS re/Start Praesignis Special**, and each of us brought critical skills to the table:  
- **Ken & Chirwa** designed and built the clean, functional static website—complete with menu images, booking forms, and a layout so intuitive, even a Bloodthirster could place an order.  
- **Ivyn** crafted and delivered the presentation, clearly explaining the restaurant’s pain points and how AWS solves them—calmly, confidently, and without summoning any daemons (impressive, given the stakes).  
- **My role (Sadiyah)**: I handled the **AWS deployment**—creating the S3 bucket, configuring permissions, enabling static hosting, and ensuring the site went live securely and correctly.

>  *Nurgle tried to “optimize” our workflow by corrupting one of the 44 image files during upload. S3 Versioning rolled it back before anyone noticed. Crisis averted.*

---

##  My AWS Deployment: Behind the Scenes  

Here’s what I did in the AWS Console:  

1. **Created an S3 bucket** named `project1-the-gourmet-grille`  
   → Remember: S3 names are **globally unique**—like naming a daemon prince. Choose wisely.  

2. **Enabled ACLs** and set **Object Ownership** to *Bucket owner preferred*  
   → This let us make **individual files public** without exposing the whole bucket.  

3. **Disabled “Block All Public Access”** (after acknowledging the warning!)  
   → Because yes—we *want* hungry customers to see the truffle pasta.  

4. **Enabled versioning**  
   → Safety net for accidental uploads (or overly enthusiastic teammates).  

5. **Uploaded `index.html` + 44 assets** via the S3 console  

6. **Enabled Static Website Hosting**  
   → Set `index.html` as the index document → got our public URL  

7. **Made all files public using ACLs**  
   → Selected everything → **“Make public using ACL”** →  **Website live!**

>  *Tzeentch whispered: “What if the URL changes mid-presentation?”  
> Ivyn’s flawless delivery proved even fate bows to good preparation.*

---

##  Why This Matters  

Before AWS, The Gourmet Grille relied on **paper bookings, phone calls, and error-prone spreadsheets**—leading to double-bookings, lost orders, and stressed staff.  

Now?  
 Customers can view the menu online  
 Booking/order forms reduce human error  
 Owner gains visibility and control  
 All hosted on **secure, scalable, pay-as-you-go AWS infrastructure**

---

##  Final Thought  

Great tech isn’t built alone—it’s built by **teams who trust each other’s strengths**.  
Ken and Chirwa built something beautiful.  
Ivyn made the business case shine.  
And I made sure it all worked in the cloud—securely, reliably, and ready to grow.

If a Chaos God tries to DDoS the dessert menu?  
We’ve got **S3 durability**, **ACL discipline**, and **a team that’s seen worse**. 

— **Sadiyah Grobbler**  
*AWS re/Start Student | Cloud Deployer | Team Player*  
*Role: AWS Infrastructure & Deployment | Tools: Amazon S3, ACLs, Static Website Hosting*
