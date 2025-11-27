# 🐧 Linux Administration on AWS

This section elaborates on the core Linux administration topics, essential commands, and the practical goals of each lab based on your course outline.

---

## 1. Introduction & Command Line Basics

### 📑 Topics & Summaries
* **Introduction to Linux:** Understanding the **Linux Kernel**, distributions (like the **Amazon Linux AMI**), and the architecture. Learn the distinction between the **GUI** and the **Command Line Interface (CLI)**.
* **Linux Command Line:** Mastering basic navigation and command structure. This is the foundation for all subsequent work.

### 💻 Essential Codes & Uses
| Code/Command | Use & Context |
| :--- | :--- |
| `ls -al` | Lists directory contents, including hidden files, in **long format**. Key for checking file details. |
| `cd ~` / `cd -` | Navigates to the **home directory** (`~`) or back to the **previous directory** (`-`). |
| `pwd` | Prints the **Working Directory**. Essential for confirming your current location. |
| `echo $PATH` | Displays the **environment variable** that stores directories searched for executable commands. |


---

## 2. Users, Groups, and Permissions

### 📑 Topics & Summaries
* **Linux Users and Groups:** Understanding the concepts of **root** (superuser), standard users, and user groups. Learn how Linux uses groups to manage access permissions efficiently.
* **Managing Linux File Permissions:** Deep dive into the **read (r), write (w), and execute (x)** bits. Learn both **symbolic (u+rwx)** and **octal (755)** notation for setting permissions. This is critical for security. 

### 💻 Essential Codes & Uses
| Code/Command | Use & Context |
| :--- | :--- |
| `sudo useradd -m username` | Creates a new user with a **home directory** (`-m`). |
| `sudo passwd username` | Sets or changes the password for an existing user. |
| `sudo usermod -aG groupname username` | **A**dds a user to an existing **G**roup (e.g., adding a user to the `wheel` or `sudo` group). |
| `chmod 644 file.txt` | Sets permissions: **Owner: read/write (6)**, **Group/Others: read-only (4)**. |
| `chown user:group file` | **Change Owner** and Group of a file or directory. |


---

## 3. File System, Editing, and Management

### 📑 Topics & Summaries
* **Working with the Linux File System:** Understanding key directories like `/etc` (config), `/var` (variable data/logs), `/home`, and `/opt`. Learn about device files and mounting.
* **Editing Files in Linux:** Essential for configuration. Focus on the use of simple text editors like **`nano`** for beginners or **`vim`/`vi`** for power users.
* **Working with Files in Linux:** Focus on file location, searching, comparing, and archiving.

### 💻 Essential Codes & Uses
| Code/Command | Use & Context |
| :--- | :--- |
| `nano /etc/ssh/sshd_config` | Opens the SSH daemon configuration file for editing. |
| `find / -name "filename"` | **Searches** the entire filesystem (`/`) for a file by name. |
| `grep "pattern" file.txt` | **Searches** for a string of text within a file. |
| `tar -czvf archive.tar.gz /path` | **Archives** and **compresses** a directory (`c`reate, `z`ip, `v`erbose, `f`ile). |
| `df -h` / `du -sh` | **D**isk **F**ree (capacity) / **D**isk **U**sage (file size) in **h**uman-readable format. |

---

## 4. System Operations and Automation

### 📑 Topics & Summaries
* **Working with Linux Commands:** Advanced command usage, including piping (`|`), chaining (`&&`), command substitution, and aliases.
* **Managing Linux Processes:** Understanding **Process IDs (PIDs)**, parent/child relationships, and process states. Learn how to monitor and terminate processes.
* **Managing Linux Services:** Focus on **`systemd`** and the **`systemctl`** command for managing background applications/daemons (e.g., Apache, Nginx).
* **The Bash Shell & Bash Shell Scripting:** Introduction to writing simple **scripts** (`.sh` files) to automate multi-step tasks. Includes variables, conditional logic (`if/then`), and loops (`for`).
* **Linux Software Management:** Using package managers like **`yum`** (on RHEL/Amazon Linux) or `apt` (on Ubuntu/Debian) to install, update, and remove software packages.
* **Managing Linux Log Files:** Understanding the importance of logs in `/var/log` for troubleshooting. Focus on using commands like **`tail`**, **`less`**, and log rotation concepts.

### 💻 Essential Codes & Uses
| Code/Command | Use & Context |
| :--- | :--- |
| `ps aux` | Shows all running processes for all users. |
| `kill -9 PID` | **Terminates** a runaway process immediately (forcibly). |
| `systemctl status httpd` | Checks the **status** of the Apache web server service. |
| `yum install httpd -y` | Installs the Apache package and **automatically answers yes** (`-y`). |
| `tail -f /var/log/messages` | **Continuously displays** new lines added to a log file ("follow"). Critical for real-time monitoring. |
| `./script.sh` | Executes a bash script located in the current directory. |

### 🧪 Labs (Goals)

* **253-[LX]-Lab - [Challenge] Bash Shell Scripting:** Goal is to write a complex script that **automates a multi-step task**, such as installing a package, creating a configuration file, and restarting a service.
