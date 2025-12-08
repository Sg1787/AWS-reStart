# How to Write a Simple Backup Script in Bash (Linux on AWS)

This guide walks you through creating a reusable Bash script that **automatically backs up a folder** with a **timestamped filename**—just like you’d do in real sysadmin work!

---

##  What Your Script Does
- Compresses the `CompanyA/` folder into a `.tar.gz` archive.
- Names the backup like: `2025_12_09-backup-CompanyA.tar.gz`.
- Saves it to a `backups/` directory in your home folder.
- Ready to be scheduled with `cron` for daily backups!

---

## Step-by-Step: Create the Script

### 1. **Connect to your EC2 instance**
```bash
ssh -i labsuser.pem ec2-user@<your-public-ip>

