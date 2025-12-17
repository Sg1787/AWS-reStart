# Working with Amazon EBS  
*Making Data Stick in a World That Vanishes*

> *"EC2 instances die. But your data? That stays."*  
> — Sadiyah Grobbler

<img width="1163" height="349" alt="image" src="https://github.com/user-attachments/assets/66bbfd91-8160-46ad-a904-ecbc07257237" />

In this lab, I learned how to give an EC2 instance **permanent storage** using **Amazon EBS**—so even if the server shuts down, my files don’t disappear. I created a volume, attached it, saved a file, backed it up with a **snapshot**, and later **restored it** exactly as it was.

Here’s how I did it:

---

##  Step 1: Create an EBS Volume

- Went to **EC2 > Volumes**
- Created a new **1 GiB General Purpose SSD (gp2)** volume
- Made sure it was in the **same Availability Zone** as my `Lab` EC2 instance (`us-west-2a`)
- Tagged it with **Name: My Volume**
<img width="1632" height="427" alt="image" src="https://github.com/user-attachments/assets/d2fd1b1d-3849-4aa2-a87d-ad169f2f3546" />
<img width="1673" height="143" alt="image" src="https://github.com/user-attachments/assets/34782a91-fb73-4e90-bb13-d648372c59a9" />


>  *EBS volumes can only attach to instances in the same AZ—like trying to plug in a USB cable across cities. It won’t work.*

---

##  Step 2: Attach the Volume to the EC2 Instance

- Selected **My Volume** → **Actions > Attach volume**
- Chose the **Lab** instance
- Set the **device name** to `/dev/sdb` (important for later!)
- Volume status changed to **In-use**

---

##  Step 3: Connect and Set Up the File System

Used **EC2 Instance Connect** to log in to the `Lab` instance, then ran these commands:
<img width="1704" height="606" alt="image" src="https://github.com/user-attachments/assets/3e2f821b-8b9c-4f90-8518-4cb5e38664aa" />

```bash
# Check current disks (my new volume won't show yet)
df -h

# Format the new volume with ext3 file system
sudo mkfs -t ext3 /dev/sdb

# Create a folder to mount it
sudo mkdir /mnt/data-store

# Mount the volume
sudo mount /dev/sdb /mnt/data-store

# Make it mount automatically after reboot
echo "/dev/sdb   /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab

# Save a test file
sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"

# Check it worked
cat /mnt/data-store/file.txt
```

 Output: `some text has been written`
<img width="1026" height="665" alt="image" src="https://github.com/user-attachments/assets/002321c2-0be9-413b-b994-bb6f46c631a5" />

---

## 📸 Step 4: Create a Snapshot (Your Safety Net)

Back in the AWS Console:
- Selected **My Volume** → **Actions > Create snapshot**
- Tagged it: **Name: My Snapshot**
- Waited for status to change from **Pending** → **Completed**
<img width="1178" height="146" alt="image" src="https://github.com/user-attachments/assets/260eec64-edab-40b4-ace4-7a99cabb2357" />


Then, **deleted the test file** to prove the snapshot really works:
```bash
sudo rm /mnt/data-store/file.txt
ls /mnt/data-store/file.txt  # → "No such file"
```
<img width="624" height="70" alt="image" src="https://github.com/user-attachments/assets/88b00428-9145-4207-9287-219cf505415f" />

---

##  Step 5: Restore from Snapshot

### 5.1 Create a New Volume from the Snapshot
- Selected **My Snapshot** → **Actions > Create volume from snapshot**
- Kept the same **Availability Zone**
- Tagged it: **Name: Restored Volume**

### 5.2 Attach the Restored Volume
- Attached to the **Lab** instance as `/dev/sdc`

### 5.3 Mount and Recover the File
```bash
sudo mkdir /mnt/data-store2
sudo mount /dev/sdc /mnt/data-store2
ls /mnt/data-store2/file.txt  # → file.txt FOUND!
```

 The file was back—exactly as I left it.

---

##  What I Learned

- EBS gives **persistent storage** for EC2 instances.
- Volumes must be in the **same AZ** as the instance.
- **Snapshots** back up only used data and live in S3 (but you don’t manage that).
- You can **restore snapshots** to new volumes—even in different AZs or with bigger sizes.
- **`/etc/fstab`** makes your volume auto-mount on reboot.

> *"In the cloud, nothing lasts—unless you make it."*

— Sadiyah Grobbler  
**AWS Re/Start Learner | Future Security Engineer**



 **Real-World Tip**: In production, use **encrypted EBS volumes** and **automated snapshots** for backup and compliance.
