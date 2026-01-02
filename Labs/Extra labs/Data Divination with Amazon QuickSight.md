# Project: Data Divination with Amazon QuickSight  
### *“All data bends to the will of the Dark Pantheon.”*

> *“Knowledge is power, guard it well… but first, visualize it.”*  
> — Adapted from the Tome of Tzeentch

In this ritual, I summoned **Amazon QuickSight**—a cloud-born oracle—to **gaze into the screaming void of raw data** and shape its chaos into **truth, beauty, and actionable heresy**. From CSV tombs in S3 to living dashboards that whisper secrets of release years, genres, and mortal entertainment habits, this project is a testament to **data sorcery done right**.

All hail the **Daemons of Insight**!

---

##  What is Amazon QuickSight?

A **cloud-scale business intelligence (BI) service** that transforms tangled data into **clear, shareable visions**—delivered across the galaxy with but a thought.

- Connects to data in the cloud  
- Merges sources like a **Tzeentchian schemer weaving fate**  
- Renders insights so elegant, even **Slaanesh would weep**

---

##  Before the Ritual: Prepare the Offering

To feed the oracle, I placed two sacred artifacts into an **S3 void-cyst**:

- `dataset.csv` — the **raw flesh of truth**, bloated with records of films and shows  
- `manifest.json` — the **soul-binding contract** that tells QuickSight *where* the data lies  

>  **Critical Note**:  
> The `manifest.json` **must contain the correct S3 URI**. An outdated path is like a broken pact—**the daemon will not awaken**.

I also **granted QuickSight explicit permission** to peer into my S3 bucket. Even daemons require invitation… or at least IAM policy.

---

##  The Sixfold Path of Visualization

### **Step 1: Summon the QuickSight Oracle**  
- Created a **QuickSight account** (free tier, blessed by the Omnissiah).  
- Took **less than two minutes**—faster than a Khornate berserker’s charge.
<img width="604" height="229" alt="image" src="https://github.com/user-attachments/assets/dd47bd83-f93e-4dab-a7f7-50fcf35c1ff2" />



### **Step 2: Bind S3 to the Oracle**  
- Connected QuickSight to my S3 bucket using the **manifest.json** as a **summoning sigil**.  
- Without this file, the oracle stares blankly into the void. *With it*, the data **screams its secrets**.
<img width="679" height="259" alt="image" src="https://github.com/user-attachments/assets/f2292065-8339-4914-bfd7-1351f03bfb41" />


### **Step 3: Invoke the First Vision**  
- Entered the **manifest URL** into QuickSight.  
- Dragged `release_year` into the Y-axis—**and lo, a chart was born!**  
  → A **barrow-count of media by year**, rising like Nurgle’s ever-growing boils.
<img width="753" height="285" alt="image" src="https://github.com/user-attachments/assets/f15e3065-a6ee-4aad-8ad3-bd88f7faff2d" />



### **Step 4: Refine the Revelation**  
- Filtered for **Action, Adventure, Thriller, and TV Comedy**—the favored genres of mortal distraction.  
- The dashboard now **sings in Slaaneshi harmony**: clean lines, vivid hues, intoxicating clarity.
<img width="414" height="297" alt="image" src="https://github.com/user-attachments/assets/091e7d6f-9018-4433-bc12-92f32b2674d0" />


### **Step 5: Purge the Unworthy (Filters!)**  
- Cast out all records **before 2015**—**weak, outdated, irrelevant**.  
- Only the **strongest data** remains, like warriors surviving the Blood Games of Khorne.
<img width="442" height="238" alt="image" src="https://github.com/user-attachments/assets/3ab7c6e4-369b-436e-bd69-ce227986f3fb" />


### **Step 6: Publish the Gospel**  
- Named and **published the dashboard**—now a **public shrine of insight**.  
- Exported as **PDF** using the **“Export” sigil** (top-right corner).  
- Polished labels and totals for **mortal readability**—because even heresy must be *understood*.
<img width="1090" height="731" alt="image" src="https://github.com/user-attachments/assets/d91272e3-c8fd-480d-9663-66f17ae256f5" />

---

##  The Final Dashboard

Behold! A **living tapestry of data**, woven from chaos and rendered in divine clarity:

- Tracks genre popularity post-2015  
- Reveals release-year trends  
- Serves as a **foundation for future divinations**

>  **Future Heresies**:  
> - Connect to **Athena** or **RDS** for real-time data feasts  
> - Trigger **Lambda daemons** on dashboard interaction  
> - Embed dashboards into **S3-hosted websites** (like The Gourmet Grille!) for **omnichannel blasphemy**

---

##  Troubleshooting (Warp Warnings)

- **"Access Denied" to S3?** → Check bucket policy + QuickSight permissions.  
- **Manifest not loading?** → Verify S3 URI is **publicly accessible** and **correctly formatted**.  
- **Blank visualizations?** → Ensure fields are **dragged into the correct axis bins**—Tzeentch mocks the careless.

---

> *“I did not expect QuickSight to be so… mortal-friendly.”*  
> — Sadiyah Grobbler, Daemon-Engineer & Data Diviner

*Documented in service to the Dark Mechanicum and the Four Thrones of Chaos.*  
🜂 **GitHub**: [@Sg1787](https://github.com/Sg1787)  
🜂 **Thanks to NextWork** for the sacred project guide!

*Let the data rot. Let it bloom. Let it reveal.* 
