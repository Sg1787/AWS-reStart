#  My AWS CLI Setup Notes (Red Hat)

## Goal
My goal was to install and set up the **AWS CLI** on a Red Hat Linux instance and use it to interact with **IAM**.
<img width="1017" height="379" alt="image" src="https://github.com/user-attachments/assets/22c407f9-9352-4743-aa76-4f7d78afde8c" />

The diagram shows the simple path a user can take to manage AWS services:

1. The User connects to the Red Hat EC2 instance using SSH.
2. The user runs the AWS CLI tool on that server.
3. The AWS CLI sends commands to manage AWS services, like IAM.

---

##  Task 1: I Connected via SSH

I used the **PublicIP** and my `labsuser.pem` key to connect from my terminal.

<img width="442" height="440" alt="image" src="https://github.com/user-attachments/assets/49d1dc05-8702-4ae0-8563-2b084eaa91b8" />

<img width="635" height="412" alt="image" src="https://github.com/user-attachments/assets/633fe468-83e1-4b68-804e-8441753209a1" />


---

##  Task 2: I Installed the AWS CLI

I ran these four commands in the SSH terminal:

1.  **I downloaded the ZIP:**
    ```bash
    curl "[https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip](https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip)" -o "awscliv2.zip"
    ```
2.  **I unzipped the file:**
    ```bash
    unzip -u awscliv2.zip
    ```
3.  **I ran the installer:**
    ```bash
    sudo ./aws/install
    ```
4.  **I checked the version:**
    ```bash
    aws --version
    ```
<img width="697" height="82" alt="image" src="https://github.com/user-attachments/assets/b2c8f9a1-a506-4d24-b494-e4ae8ebc605c" />

---
##  Task 3: I Checked IAM Configuration in the AWS Console

My goal was to look at the permissions for the user I'm using.

1.  **I searched** for and chose **IAM** in the AWS Console search bar.
2.  In the navigation pane, I went to **Users** and selected **awsstudent**.
<img width="938" height="255" alt="image" src="https://github.com/user-attachments/assets/6e2419ed-6e04-4d3b-ac54-9673c1d4f12e" />

   
4.  **I viewed the Policy:** I checked the **Permissions** tab, expanded the `lab_policy`, and chose the **{} JSON** button to see the policy document.
    * *(This JSON document shows which AWS services the user can access.)*
<img width="1218" height="668" alt="image" src="https://github.com/user-attachments/assets/e879f14f-1313-4c2d-9ce9-d518f88208d4" />

5.  **I located the Access Key:** I switched to the **Security credentials** tab and found the user's **Access key ID**.
    * *(Note: The Secret Key is found in the "Details" dropdown for the lab.)*
<img width="876" height="227" alt="image" src="https://github.com/user-attachments/assets/2aaa96ef-2f55-4054-913c-e94a6610e287" />

--

##  Task 4: I Configured the AWS CLI

I used my **Access Key ID** and **Secret Key** (from the Details panel) to configure the CLI.

1.  **I started the setup:**
    ```bash
    aws configure
    ```
2.  **I entered the details:**
    * AWS Access Key ID: *[Pasted Key]*
    * AWS Secret Access Key: *[Pasted Key]*
    * Default region name: `us-west-2`
    * Default output format: `json`
<img width="662" height="66" alt="image" src="https://github.com/user-attachments/assets/0309e253-6fcf-49ba-a7ee-aa69f45c8482" />


---

##  Task 5: I Tested IAM Access

I verified my connection by listing IAM users.

1.  **I ran the test command:**
    ```bash
    aws iam list-users
    ```
    *Result: I saw a JSON list of IAM users.*
<img width="709" height="186" alt="image" src="https://github.com/user-attachments/assets/7b7ac292-f4f0-49e9-b708-52449df1d85d" />

---

## Challenges

1.  **I struggled to find the Security credentials tab

---

## Summary
I successfully installed, configured, and tested the AWS CLI.
