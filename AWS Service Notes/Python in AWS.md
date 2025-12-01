# 🐍 Simple Python & AWS DevOps Cheat Sheet ☁️

This document explains key Python ideas and how they are used to **automate tasks in the AWS Cloud** using the **Boto3** library.

---

## 💻 I. Core Python Programming

This section covers the building blocks of writing scripts.

| Topic | Simple Summary | Core Code Snippet |
| :--- | :--- | :--- |
| **Introduction to Python** | Basic setup check and output. | `print("Hello world")` |
| **Data Types** | Numbers (`10`, `3.14`) and Text (`"Hello"`). | `age = 30` |
| **List, Tuple, Dictionary** | **List:** An ordered, **changeable** box of items. **Tuple:** An ordered, **fixed** box of items. **Dict:** A lookup book using `key: value` pairs (like AWS responses). | `data = {'name': 'Insulin', 'charge': 5}` |
| **Flow Control** | **Conditionals (`if`):** Used to make decisions. **Loops (`for`, `while`):** Used to repeat actions. | `for item in list: print(item)` |
| **Application of Flow Control** | Using loops to calculate results (like net charge) or process sequences of data. | `total = 0; for c in charges: total += c` |
| **Functions** | A **reusable mini-program**. Write a task once and call it many times. | `def status_check(id): return True` |
| **Caesar Cipher** | **Example:** Using functions to wrap up complex, reusable logic (like encryption). | `def caesar(text, shift): # logic here` |

---

## 📚 II. System Tools & Modules

| Topic | Simple Summary | Core Code Snippet |
| :--- | :--- | :--- |
| **Modules and Libraries** | Importing pre-written code collections (tools) to extend what Python can do (e.g., talk to AWS). | `import boto3` |
| **File Handlers** | Reading information *from* or writing results *to* local files (like configuration or logs). | `with open('config.json', 'r') as f: data = json.load(f)` |
| **Python for System Administration** | Using Python instead of basic shell scripts to manage the computer's operating system (files, processes, users). | `import os; os.system('ls -l')` |

---

## 🐞 III. Finding Mistakes (Debugging & Testing)

| Practice | Simple Summary & Purpose | Core Code Snippet |
| :--- | :--- | :--- |
| **Using the Debugger (`pdb`)** | **Debugging:** A tool that **pauses your script** so you can check variables line-by-line to find mistakes. | `import pdb; pdb.set_trace()` |
| **Debugging Application** | Using debugger commands (`n` for next, `p` for print variable) to trace data flow through a function. | `(Pdb) p variable_name` |
| **Testing (Unit Tests & Mocking)** | **Unit Tests:** Checking if small functions work right. **Mocking:** **Faking** the dangerous AWS commands (like deleting a server) when testing, so you don't break the real cloud. | `self.assertEqual(func(1), 2)` |

---

## ☁️ IV. AWS Automation (DevOps)

**Boto3 (AWS SDK for Python)** is your main tool to manage AWS from Python scripts.

| AWS Concept | Simple Summary & Use | Boto3 Code Example |
| :--- | :--- | :--- |
| **Automation vs. Orchestration** | **Automation:** A script doing a single task (like starting a server). **Orchestration:** A system (like CodePipeline) coordinating many automated tasks in a sequence. | *N/A (Conceptual)* |
| **AWS Lambda** | **Serverless code.** Runs Python scripts automatically when triggered by an event (like a file upload) without needing a server setup. | `lambda_client = boto3.client('lambda')` |
| **CI/CD (CodePipeline)** | Automating the entire process of **Build, Test, and Deploy** software reliably and quickly. | *Scripts run as part of the pipeline steps.* |
| **Infrastructure as Code (IaC)** | Managing your cloud setup (servers, databases, networks) using **text files** (like CloudFormation or Terraform) instead of manual clicking. | `cfn = boto3.client('cloudformation'); cfn.create_stack(...)` |
| **Python Configuration Management** | Using Python and Boto3 to enforce specific settings on your cloud resources (e.g., tagging all instances, updating security firewall rules). | `ec2 = boto3.client('ec2'); ec2.stop_instances(InstanceIds=['i-1234'])` |
