#  Project 1: Host "The Gourmet Grille" on Amazon S3  
### *(A Nurgle-Blessed Offering to the Cloud-Omnissiah)*

> *"Let it fester. Let it grow. Let it serve.*  
> *For in entropy, there is sustenance."*  
> — The Seventy-Seven Precepts of Grandfather Nurgle

In this ritual, I have seeded **"The Gourmet Grille"**—a humble feast-hall of static pages—into the **fertile cysts of Amazon S3**. No servers to perish. No databases to bleed. Just **eternal, self-replicating content**, bloated with images and served openly to all who hunger.

Like a Plaguebearer’s boil, this website **swells with purpose**, grows with grace, and **never truly dies**—only evolves in glorious, versioned decay.

---

##  Step 1: Create the Blessed Cyst (S3 Bucket)

> *"Every bucket is a new lesion upon the flesh of the cloud—unique, irreplaceable, and divinely yours."*

1. Log in to the **AWS Console** (the cathedral of mortal infrastructure).
2. Search for **S3**—the sacred void-vault.
<img width="1010" height="204" alt="image" src="https://github.com/user-attachments/assets/56e89df4-3c65-4f62-87f5-01a57be26113" />


3. Select the **AWS Region closest to your mortal coil** (top-right corner).
4. Click **Create bucket**.
<img width="1090" height="228" alt="image" src="https://github.com/user-attachments/assets/2450c946-41cd-4954-99f7-648b5026d990" />

5. Name your cyst:  
   ```
   project1-thegourmetgrille
   ```  
   >  **Nurgle’s Whisper**:  
   > This name is **globally unique**—like a pustule on the skin of reality. Once formed, **no other soul in the multiverse** may claim it. Choose wisely, for deletion is the only release.
<img width="1090" height="204" alt="image" src="https://github.com/user-attachments/assets/b345d8c7-a96a-41b1-b689-f961e87737c4" />

6. Under **Object Ownership**, choose **ACLs enabled**.  
   >  **Why ACLs?**  
   > ACLs are the **pus-filled sigils** that dictate who may touch your sacred objects. Unlike blunt bucket policies, ACLs let **each file fester in its own way**—some public, some hidden, all part of the Grand Design.
<img width="1090" height="320" alt="image" src="https://github.com/user-attachments/assets/a3735170-5652-4fb7-a4c1-5ef486029013" />


7. Select **Bucket owner preferred**.
8. **Uncheck** _Block all public access_.  
   >  **The Yellow Omen Appears!**  
   > This warning is **not a curse—but a blessing**. It means your cyst may bloom openly—a **public shrine** for all to witness. *Exactly as Nurgle intends.*

9. Acknowledge the truth:  
   > _“I acknowledge that the current settings might result in this bucket and the objects within becoming public.”_
10. Enable **Bucket Versioning**—for every change is a new stage of glorious rot.
11. Click **Create bucket**.
<img width="1090" height="317" alt="image" src="https://github.com/user-attachments/assets/be8b2ade-fde4-490b-b6bd-770b03bd82b2" />
<img width="1090" height="113" alt="image" src="https://github.com/user-attachments/assets/34969154-dcfc-4146-93e8-7d605c577360" />


The cyst is formed. Let it swell.

---

##  Step 2: Seed the Cyst with Sacred Flesh (Upload Files)

> *"Feed the boil. Nurture the growth. Let the feast begin."*

1. Open your bucket: `project1-thegourmetgrille`.
<img width="1090" height="274" alt="image" src="https://github.com/user-attachments/assets/93873318-0b04-4720-9741-d3637fac92d9" />

2. Go to **Objects** → **Upload**.
<img width="1090" height="263" alt="image" src="https://github.com/user-attachments/assets/209f948c-d95d-48db-9abe-f91ce940f552" />
<img width="1090" height="271" alt="image" src="https://github.com/user-attachments/assets/292b1c2b-be04-4598-b0c4-d12ddfae9047" />


3. Add `index.html`—the **heart of the feast**.
4. Add the `images/` folder—**44 morsels of visual decay**.
5. Confirm upload of all files.
6. Click **Upload**, then **Close** when the green banner appears.
<img width="1090" height="442" alt="image" src="https://github.com/user-attachments/assets/5a69f88f-3c6d-4713-a031-faa39bb4615d" />

 The cyst pulses with content. It is **alive**.

---

##  Step 3: Awaken the Public Shrine (Static Website Hosting)

> *"A feast unseen is a plague unspread."*

1. Go to **Properties**.
2. Scroll to **Static website hosting** → **Edit**.
3. Configure:
   -  **Enable**
   - **Hosting type**: _Host a static website_
   - **Index document**: `index.html`
4. **Save changes**.
5. Copy the **Bucket website endpoint** URL.
<img width="1049" height="672" alt="image" src="https://github.com/user-attachments/assets/32500e3b-c90b-47cd-b04b-0bfc8cb60226" />

>  **Why the 403 Forbidden?**  
> The shrine is built—but the **offerings remain veiled**. Like a boil still sealed, the files are **private by default**.  
> To spread the gospel of The Gourmet Grille, you must **rupture the cyst** and let its contents flow freely.
<img width="1090" height="303" alt="image" src="https://github.com/user-attachments/assets/1d4ca61d-d90b-46ed-83ee-6f86115872be" />

---

##  Step 4: Rupture the Cyst (Make Public)

> *"Release the blessings! Let all partake in the feast of decay!"*

1. In **Objects**, select:
   - `index.html`
   - `images/` folder
2. **Actions** → **Make public using ACL** → **Make public**.
<img width="439" height="791" alt="image" src="https://github.com/user-attachments/assets/350987e0-23bd-48f0-9073-c70c73b0240b" />
<img width="1090" height="300" alt="image" src="https://github.com/user-attachments/assets/d6b9a91f-ea01-422c-8068-5bc0b9494d1c" />

3. Close the success banner.
4. **Refresh** your browser tab showing the 403 error.

 **Behold!** The website blooms in full, **public, glorious rot**—served endlessly from the eternal vaults of S3.
<img width="1090" height="963" alt="image" src="https://github.com/user-attachments/assets/b5840949-70a0-4ca8-a692-6e094f81aefc" />

---

*Documented in devotion by **Sadiyah Grobbler**,  
Bearer of Bloated Code, Tended of Data-Cysts, and Humble Disciple of the Plague God’s Cloud.*  
🜂 *"Do not fear decay—embrace the bloat."* 🜂
