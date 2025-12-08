# Linux Administration on AWS

This section elaborates on the core Linux administration topics, essential commands, and the practical goals of each lab based on your course outline.

---

## 1. Introduction & Command Line Basics

###  Topics & Summaries
* **Introduction to Linux:** Understanding the **Linux Kernel**, distributions (like the **Amazon Linux AMI**), and the architecture. Learn the distinction between the **GUI** and the **Command Line Interface (CLI)**.
* **Linux Command Line:** Mastering basic navigation and command structure. This is the foundation for all subsequent work.

###  Essential Codes & Uses
| Code/Command | Use & Context |
| :--- | :--- |
| `ls -al` | Lists directory contents, including hidden files, in **long format**. Key for checking file details. |
| `cd ~` / `cd -` | Navigates to the **home directory** (`~`) or back to the **previous directory** (`-`). |
| `pwd` | Prints the **Working Directory**. Essential for confirming your current location. |
| `echo $PATH` | Displays the **environment variable** that stores directories searched for executable commands. |


---

## 2. Users, Groups, and Permissions

###  Topics & Summaries
* **Linux Users and Groups:** Understanding the concepts of **root** (superuser), standard users, and user groups. Learnt how Linux uses groups to manage access permissions efficiently.
* **Managing Linux File Permissions:** Deep dive into the **read (r), write (w), and execute (x)** bits. Learn both **symbolic (u+rwx)** and **octal (755)** notation for setting permissions. This is critical for security. 

###  Essential Codes & Uses
| Code/Command | Use & Context |
| :--- | :--- |
| `sudo useradd -m username` | Creates a new user with a **home directory** (`-m`). |
| `sudo passwd username` | Sets or changes the password for an existing user. |
| `sudo usermod -aG groupname username` | **A**dds a user to an existing **G**roup (e.g., adding a user to the `wheel` or `sudo` group). |
| `chmod 644 file.txt` | Sets permissions: **Owner: read/write (6)**, **Group/Others: read-only (4)**. |
| `chown user:group file` | **Change Owner** and Group of a file or directory. |


---

## 3. File System, Editing, and Management

### Topics & Summaries
* **Working with the Linux File System:** learning key directories like `/etc` (config), `/var` (variable data/logs), `/home`, and `/opt`. Learnt about device files and mounting.
* **Editing Files in Linux:** Essential for configuration. Focused on the use of simple text editors like **`nano`** for beginners or **`vim`/`vi`** for power users.
* **Working with Files in Linux:** Focused on file location, searching, comparing, and archiving.

### Essential Codes & Uses
| Code/Command | Use & Context |
| :--- | :--- |
| `nano /etc/ssh/sshd_config` | Opens the SSH configuration file for editing. |
| `find / -name "filename"` | **Searches** the entire filesystem (`/`) for a file by name. |
| `grep "pattern" file.txt` | **Searches** for a string of text within a file. |
| `tar -czvf archive.tar.gz /path` | **Archives** and **compresses** a directory (`c`reate, `z`ip, `v`erbose, `f`ile). |
| `df -h` / `du -sh` | **D**isk **F**ree (capacity) / **D**isk **U**sage (file size) in **h**uman-readable format. |

---

## 4. System Operations and Automation

### Topics & Summaries
* **Working with Linux Commands:** Advanced command usage, including piping (`|`), chaining (`&&`), command substitution, and aliases.
* **Managing Linux Processes:** Understanding **Process IDs (PIDs)**, parent/child relationships, and process states. Learn how to monitor and terminate processes.
* **Managing Linux Services:** Focus on **`systemd`** and the **`systemctl`** command for managing background applications/daemons (e.g., Apache, Nginx).
* **The Bash Shell & Bash Shell Scripting:** Introduction to writing simple **scripts** (`.sh` files) to automate multi-step tasks. Includes variables, conditional logic (`if/then`), and loops (`for`).
* **Linux Software Management:** Using package managers like **`yum`** (on RHEL/Amazon Linux) or `apt` (on Ubuntu/Debian) to install, update, and remove software packages.
* **Managing Linux Log Files:** Understanding the importance of logs in `/var/log` for troubleshooting. Focus on using commands like **`tail`**, **`less`**, and log rotation concepts.

### Essential Codes & Uses in  Linux Administration on AWS – Friendly Guide

Think of this as your go-to companion for learning Linux the practical way—right inside AWS.  
No jargon overload. Just clear explanations, useful commands, and *why* they matter.

---

## 1. Getting Started & The Command Line

### What I Learnt
- **What is Linux?** It’s not just an OS—it’s a powerful, open-source foundation used by millions of servers (including yours in AWS!).  
  → You’ll use **Amazon Linux** (a lightweight, secure distro made by AWS).
- **CLI vs. GUI**: On servers, there’s no mouse or desktop—just the **command line**. It’s faster, more powerful, and perfect for automation.

### Daily Commands 
| Command | What It Does (In Plain English) |
|--------|-------------------------------|
| `ls -al` | Show me **everything** in this folder—including hidden files—with details like size and permissions. |
| `cd ~` | Take me **home** (your user’s main directory). |
| `cd -` | Take me back to **where I just was**. Super handy! |
| `pwd` | Tell me **exactly where I am** right now. |
| `echo $PATH` | Show me which folders the system checks when I type a command (like `ls` or `python`). |

---

## 2. Users, Groups & Permissions (The Security Stuff)

### What I Learnt
- Linux is **multi-user by design**. Every file and folder has an **owner**, a **group**, and **permissions**.
- **Root** is the “admin” of the system—but you should avoid using it directly. Instead, use `sudo` when needed.
- Permissions = **Who can read, write, or run** a file. Get this wrong, and your server could be vulnerable.

### Key Commands for Safety & Control
| Command | Real-World Use |
|--------|----------------|
| `sudo useradd -m alice` | Create a new user named **alice** and give her a home folder (`/home/alice`). |
| `sudo passwd alice` | Set (or reset) **alice’s password**. |
| `sudo usermod -aG sudo alice` | Add alice to the **sudo group** so she can run admin commands. |
| `chmod 644 report.txt` | Let **you** edit the file, but others can only **read** it. (6 = read+write, 4 = read-only) |
| `chown alice:developers app.log` | Make **alice** the owner, and assign the file to the **developers** group. |

> **Golden Rule**: Give users the *least privilege* they need to do their job. Never `chmod 777` unless you *really* know what you’re doing!

---

## 3. Files, Folders & The Linux File System

### What was learnt:
- Linux has a **standard folder structure**:
  - `/etc` → Configuration files (like settings for your apps)
  - `/var/log` → Logs (your server’s diary)
  - `/home` → User directories
  - `/opt` → Optional or third-party software
- You’ll **edit config files**, **search for logs**, and **compress backups**—all from the terminal.

### Must-Know File Commands
| Command | Why It Matters |
|--------|----------------|
| `nano /etc/ssh/sshd_config` | Safely edit the SSH settings (e.g., to disable root login). `nano` is beginner-friendly! |
| `find / -name "error.log"` | “Where’s that file?” → This searches the **entire system** for it. |
| `grep "ERROR" /var/log/app.log` | Find every line containing **“ERROR”** in a log file. Instant debugging! |
| `tar -czvf backup.tar.gz /home` | Pack and compress a folder into a **single `.tar.gz` file** (great for backups). |
| `df -h` | “How much disk space is left?” → Shows it in **GB/MB**, not confusing bytes. |
| `du -sh /var/log` | “How big is this folder?” → Answers in human-readable size. |

>  **VERY IMPORTANT**: Always check disk space (`df -h`) before things start mysteriously failing!

---

## 4. Keeping Things Running: Services, Scripts & Logs

### Summery:
- **Processes**: Everything running on your server (like a web server or database) is a “process.” You’ll learn to **watch**, **stop**, or **restart** them.
- **Services**: Apps that run in the background (like `nginx` or `httpd`) are managed by **`systemd`**.
- **Automation**: Instead of typing the same 10 commands every day, write a **Bash script** to do it for you!
- **Logs**: Your best friend when something breaks. They live in `/var/log`.

### Power Commands for Admins
| Command | Real-Life Example |
|--------|--------------------|
| `ps aux` | “What’s running right now?” → Shows all active processes. |
| `kill -9 1234` | Force-stop a frozen app (PID = 1234). Use sparingly! |
| `systemctl status nginx` | Is the web server **up, down, or broken?** This tells you. |
| `sudo yum install nginx -y` | Install **nginx** on Amazon Linux—`-y` skips the “Are you sure?” prompt. |
| `tail -f /var/log/nginx/access.log` | Watch **live web requests** scroll by in real time. Great for testing! |
| `./deploy.sh` | Run your custom automation script (e.g., to deploy an app). |

---

## Final Thought

Linux isn’t about memorizing commands—it’s about **understanding how your system works** so you can fix it, improve it, and automate it.  
Every expert was once a beginner who just kept typing `ls` until it made sense. 😊

> **You’ve got this!**  
> Keep this guide handy, break things on purpose (in a test EC2!), and learn by doing.

*Made for learners, by someone who once got stuck on `chmod` too.*
| Code/Command | Use & Context |
| :--- | :--- |
| `ps aux` | Shows all running processes for all users. |
| `kill -9 PID` | **Terminates** a runaway process immediately (forcibly). |
| `systemctl status httpd` | Checks the **status** of the Apache web server service. |
| `yum install httpd -y` | Installs the Apache package and **automatically answers yes** (`-y`). |
| `tail -f /var/log/messages` | **Continuously displays** new lines added to a log file ("follow"). Critical for real-time monitoring. |
| `./script.sh` | Executes a bash script located in the current directory. |

### Labs (Goals)

### Lab Goal: Bash Scripting Challenge
>  
> **Your mission**: Write a script that:
> 1. Installs a web server (`nginx`)
> 2. Creates a simple HTML file in `/var/www/html`
> 3. Starts the service and enables it on boot  
>   
> This is **real-world automation**—the kind that saves you hours every week!
