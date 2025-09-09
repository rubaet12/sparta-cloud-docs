
<!-- TOC -->
* [AWS & SSH – Introduction](#aws--ssh--introduction)
  * [1. Introduction to AWS Virtual Machines](#1-introduction-to-aws-virtual-machines)
  * [2. Setting Up and Accessing the VM](#2-setting-up-and-accessing-the-vm)
  * [3. Why We Use SSH](#3-why-we-use-ssh)
  * [4. Key Points Recap](#4-key-points-recap)
* [Detailed Notes – EC2, SSH, and GitHub Practice](#detailed-notes--ec2-ssh-and-github-practice)
  * [1. Introduction to EC2 and SSH](#1-introduction-to-ec2-and-ssh)
  * [2. What is SSH?](#2-what-is-ssh)
  * [3. Username & Password vs SSH Keys](#3-username--password-vs-ssh-keys)
    * [SSH Key Pair](#ssh-key-pair)
    * [Advantages of SSH Keys over Passwords](#advantages-of-ssh-keys-over-passwords)
  * [4. Why Practice SSH with GitHub First?](#4-why-practice-ssh-with-github-first)
  * [5. Example Commands](#5-example-commands)
  * [Key Takeaways](#key-takeaways)
* [Cloud, Virtual Machines, and AWS EC2](#cloud-virtual-machines-and-aws-ec2)
  * [1. Virtual Machines and Virtualization](#1-virtual-machines-and-virtualization)
  * [2. AWS EC2 Overview](#2-aws-ec2-overview)
  * [3. Instance Lifecycle](#3-instance-lifecycle)
  * [4. Connecting to EC2 with SSH](#4-connecting-to-ec2-with-ssh)
  * [5. Workflow Summary](#5-workflow-summary)
  * [Key Points](#key-points)
* [Linux and AWS EC2 – Comprehensive Documentation](#linux-and-aws-ec2--comprehensive-documentation)
  * [1. Transition from EC2/SSH to Linux](#1-transition-from-ec2ssh-to-linux)
  * [2. Linux Basics](#2-linux-basics)
    * [2.1 The Linux File System](#21-the-linux-file-system)
    * [2.2 Superuser (Root Access)](#22-superuser-root-access)
    * [2.3 Moving and Renaming Files](#23-moving-and-renaming-files)
  * [3. Environment Variables](#3-environment-variables)
    * [3.1 What They Are](#31-what-they-are)
    * [3.2 Setting Variables](#32-setting-variables)
    * [3.3 Persistence](#33-persistence)
  * [4. Processes in Linux](#4-processes-in-linux)
    * [4.1 Understanding Processes](#41-understanding-processes)
    * [4.2 Viewing Processes](#42-viewing-processes)
    * [4.3 Running Processes in Background](#43-running-processes-in-background)
    * [4.4 Stopping Processes](#44-stopping-processes)
  * [5. File Permissions](#5-file-permissions)
    * [5.1 Basics](#51-basics)
    * [5.2 Viewing Permissions](#52-viewing-permissions)
    * [5.3 Modifying Permissions](#53-modifying-permissions)
  * [6. AWS EC2 Instance Lifecycle](#6-aws-ec2-instance-lifecycle)
    * [Best Practice for Sandbox Work](#best-practice-for-sandbox-work)
  * [Key Takeaways](#key-takeaways-1)
* [Bash Scripting, Debugging, and File Transfer in AWS EC2](#bash-scripting-debugging-and-file-transfer-in-aws-ec2)
  * [1. Introduction](#1-introduction)
  * [2. Bash Scripts](#2-bash-scripts)
    * [2.1 What is a Bash Script?](#21-what-is-a-bash-script)
    * [2.2 Creating and Running a Script](#22-creating-and-running-a-script)
  * [3. Using `echo` for Debugging](#3-using-echo-for-debugging)
    * [3.1 Why `echo` is Useful](#31-why-echo-is-useful)
    * [3.2 Example: Script Without `echo`](#32-example-script-without-echo)
    * [3.3 Example: Script With `echo`](#33-example-script-with-echo)
  * [4. Best Practice: Manual First, Script Later](#4-best-practice-manual-first-script-later)
    * [Process:](#process)
  * [5. Copying Files from Local to EC2](#5-copying-files-from-local-to-ec2)
    * [5.1 Why Copy Files?](#51-why-copy-files)
    * [5.2 Using `scp` (Secure Copy)](#52-using-scp-secure-copy)
    * [5.3 Alternative: Manual Creation](#53-alternative-manual-creation)
  * [6. Testing Scripts on Fresh Instances](#6-testing-scripts-on-fresh-instances)
    * [Process:](#process-1)
  * [7. EC2 Instance Lifecycle](#7-ec2-instance-lifecycle)
    * [Best Practice in Training/Sandbox Environments](#best-practice-in-trainingsandbox-environments)
* [Key Takeaways](#key-takeaways-2)
<!-- TOC -->
# AWS & SSH – Introduction

## 1. Introduction to AWS Virtual Machines

**Virtual Machine (VM):**  
- A VM is like a "machine within a machine."  
- AWS has huge servers in their data centres (e.g., 1000 CPUs, 500 GB+ RAM).  
- Renting the entire server would be too costly and unnecessary for small projects.  
- Instead, we rent a small portion of that server to create our own VM.  

**Our Example VM Specs:**  
- 2 CPUs  
- 4 GB RAM  
- Lightweight, only the resources we need.  

**How it works:**  
- A portion of the server’s hardware (CPUs, RAM) is allocated to our VM.  
- Those resources are dedicated to the VM for as long as it exists.  

---

## 2. Setting Up and Accessing the VM

- We will log into AWS over the Internet.  
- After configuring the VM, we need to log into it remotely.  
- Since the VM will run Linux without a graphical interface (GUI) (lightweight and efficient), we cannot use a desktop-style interface.  
- Instead, we must interact with the VM via the **shell (command-line interface).**  

---

## 3. Why We Use SSH

**SSH (Secure Shell):**  
- A secure protocol used to connect to remote machines.  
- Allows us to run commands and manage the VM from our laptop.  
- Provides encrypted communication, ensuring safe access over the internet.  

**Purpose here:**  
- To connect from our local laptop → AWS virtual machine.  
- To securely access the Linux shell on the VM.  
- Since no GUI is provided, the shell is the only way to control the system.  

---

## 4. Key Points Recap

- AWS gives us the ability to rent part of a powerful server as a virtual machine.  
- Our VM will be small but enough for practice (2 CPUs, 4 GB RAM).  
- To control it:  
  - We connect via the Internet.  
  - Use SSH for secure remote access.  
  - Work through the Linux shell instead of a GUI.  

---
# Detailed Notes – EC2, SSH, and GitHub Practice

## 1. Introduction to EC2 and SSH
- When starting with AWS, the first service we’ll use is **EC2** (Elastic Compute Cloud).  
- **EC2 = Virtual Machines (VMs)** hosted in AWS data centres.  
  - AWS data centres contain very powerful servers (e.g., thousands of CPUs, hundreds of GB of RAM).  
  - Instead of renting an entire server (which would be extremely expensive), AWS allows us to rent **a portion of the server** in the form of a virtual machine.  
- Our training VM example will be lightweight:  
  - ~2 vCPUs  
  - ~4 GB RAM  
- These VMs are usually **Linux-based** and run **without a graphical interface (GUI)** for efficiency.  
- Because there is no GUI, we must interact with the VM through the **command-line shell**.  
- To securely connect to the shell from our laptop over the Internet, we use **SSH (Secure Shell Protocol)**.  

## 2. What is SSH?
- **SSH (Secure Shell)** is a protocol used to connect securely to remote servers.  
- SSH provides:  
  - **Encryption** → Commands and data sent between your laptop and the server are secure.  
  - **Authentication** → Ensures only authorised users can access the server.  
  - **Remote shell access** → Lets you run commands on the remote machine as if you were sitting at it.  
- SSH is widely used for:  
  - Managing cloud servers (e.g., AWS EC2, DigitalOcean, Azure).  
  - Securely accessing services like GitHub.  
  - File transfers (via `scp` or `sftp`).  

## 3. Username & Password vs SSH Keys
- Many services (e.g., GitHub) allow login with a **username + password**.  
  - Example: Using HTTPS to push and pull from a GitHub repository with your account credentials.  
- However, SSH provides a **more secure and convenient** method of authentication using **key pairs**.  

### SSH Key Pair
- **Public Key**  
  - Stored on the server (e.g., GitHub or AWS EC2 instance).  
  - Can be shared safely.  
- **Private Key**  
  - Stored securely on your laptop.  
  - Must **never** be shared — it proves your identity.  
- **Authentication process:**  
  - When you connect to a server via SSH, the server checks if your private key matches the stored public key.  
  - If they match, you’re authenticated **without needing to type a password**.  

### Advantages of SSH Keys over Passwords
- **More secure** → Keys are much harder to brute-force than passwords.  
- **Convenient** → No need to enter your password repeatedly.  
- **Automatable** → Useful when scripts or services need to connect securely.  

## 4. Why Practice SSH with GitHub First?
- **GitHub is like AWS** in the sense that it is also a server that supports SSH connections.  
- Using GitHub to practice SSH helps us:  
  - Learn how to generate SSH key pairs (`ssh-keygen`).  
  - Configure keys locally and add them to the GitHub account.  
  - Understand how key-based authentication works.  
- Important distinction:  
  - With GitHub, SSH allows you to perform **repository actions** (push/pull/clone).  
  - GitHub does **not** provide shell access — you cannot log into GitHub servers directly.  
- Practicing with GitHub first means:  
  - You get comfortable with SSH setup in a low-stakes environment.  
  - By the time we move to AWS, you’ll already know the fundamentals of SSH.  

## 5. Example Commands
- **Generate a new SSH key pair (Linux/macOS):**  
  `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`  
  Saves a key pair in `~/.ssh/id_rsa` (private) and `~/.ssh/id_rsa.pub` (public).  
- **Add your private key to the SSH agent:**  
  `ssh-add ~/.ssh/id_rsa`  
- **Clone a GitHub repository using SSH:**  
  `git clone git@github.com:username/repo.git`  
- **Push changes to GitHub using SSH:**  
  `git push origin main`  
- **Connect to an AWS EC2 instance (example):**  
  `ssh -i my-key.pem ec2-user@ec2-3-120-45-67.compute-1.amazonaws.com`  

## Key Takeaways

- **EC2 = AWS virtual machines** → You rent part of a big server instead of the whole thing.  
- **SSH** → The secure way to log in and control your VM.  
- **GitHub practice** → A safe way to learn SSH keys before using AWS.  
- **SSH keys** → Safer and easier than passwords.  
- **Troubleshooting** → Normal and expected — part of the learning process.  
- **Training style** → Quick practice sessions followed by review time.  
- **Next step** → Launch an EC2 instance and connect with SSH.  

---

# Cloud, Virtual Machines, and AWS EC2

## 1. Virtual Machines and Virtualization
- A **virtual machine (VM)** is a simulated computer running on physical hardware.  
- It works like a real computer with its own OS but stays **isolated** from the host system.  
- **Benefits:**  
  - Runs independently of the host OS.  
  - Provides isolation and security.  
  - Allows multiple environments on the same hardware.  
- **Example:** A Windows laptop hosting a Linux VM.  

## 2. AWS EC2 Overview
- **EC2 (Elastic Compute Cloud)** is AWS’s service for creating and running VMs.  
- **Features:**  
  - Choose different operating systems.  
  - Scale CPU, RAM, and storage as needed.  
  - Pay only for what you use.  
- For our setup, use **Ubuntu** (not Amazon Linux).  

## 3. Instance Lifecycle
- EC2 instances have states:  
  - **Running** → Active and using resources (costs money).  
  - **Stopped** → Paused, can be restarted later.  
  - **Terminated** → Permanently deleted.  
- **Best practice:** Stop instances when finished instead of terminating.  

## 4. Connecting to EC2 with SSH
- EC2 is a **Linux machine without a GUI**, so we use the **shell**.  
- **SSH (Secure Shell):**  
  - Encrypts communication between your laptop and the instance.  
  - Lets you run remote commands.  
- Example command:  
  `ssh -i my-key.pem ubuntu@ec2-xx-xxx-xxx-xx.compute-1.amazonaws.com`  
  - `-i my-key.pem` → Path to private key.  
  - `ubuntu@` → Default username for Ubuntu.  
  - `ec2-xx-xxx-xxx-xx.compute-1.amazonaws.com` → Public DNS.  

## 5. Workflow Summary
1. Launch an EC2 instance in AWS (choose **Ubuntu**).  
2. Connect with SSH.  
3. Run Linux commands in the shell.  
4. **Stop** the instance when done to save it.  
5. Restart when ready to continue.  

## Key Points
- **VMs** let you run isolated OS environments.  
- **AWS EC2** provides flexible, on-demand cloud VMs.  
- Use **Ubuntu** for practice.  
- Connect securely with **SSH**.  
- **Stop** instances when not in use; only terminate if you want permanent deletion.  
---

# Linux and AWS EC2 – Comprehensive Documentation

## 1. Transition from EC2/SSH to Linux
- After creating and connecting to **AWS EC2 instances** via **SSH**, the focus moves to **Linux fundamentals**.  
- Linux is the most common operating system for cloud servers because:  
  - It is lightweight.  
  - It is stable and secure.  
  - It is widely supported in DevOps workflows.  
- Since EC2 instances typically run **Ubuntu Linux** without a **GUI (Graphical User Interface)**, interaction happens through the **command-line shell**.  

## 2. Linux Basics

### 2.1 The Linux File System
- **Root Directory (`/`)**  
  - The top-level directory in Linux.  
  - Contains all system files, configuration, and subdirectories.  
  - Comparable to “C:\” in Windows.  
- **Home Directory (`/home/username`)**  
  - Each user has their own “home” folder.  
  - Stores personal files, configuration, and documents.  
  - Example: `/home/ubuntu` for the default EC2 Ubuntu user.  
- **Important Difference**  
  - `root` directory = whole system.  
  - `home` directory = user-specific space.  

### 2.2 Superuser (Root Access)
- The **root user** (superuser) has full control over the system.  
- **Becoming superuser temporarily (recommended):**  
  `sudo <command>` → Example: `sudo apt update`  
- **Starting a superuser session:**  
  `sudo -i`  
- **Returning to a normal user:**  
  `exit`  
- **Best practice:** Use `sudo` only when needed — staying logged in as root all the time increases the risk of mistakes.  

### 2.3 Moving and Renaming Files
- Move a file: `mv file.txt /home/ubuntu/Documents/`  
- Rename a file: `mv oldname.txt newname.txt`  
- Move a directory: `mv /tmp/project /home/ubuntu/`  

## 3. Environment Variables

### 3.1 What They Are
- Variables in Linux store values for use by the system and applications.  
- **Normal variables**: Exist only in the current shell session.  
- **Environment variables**: Inherited by all processes started from the shell.  

### 3.2 Setting Variables
- Normal variable:  
  `NAME="ClientProject"`  
  `echo $NAME` → outputs `ClientProject`  
- Environment variable:  
  `export APP_ENV=production`  
  `echo $APP_ENV`  

### 3.3 Persistence
- Variables disappear when you log out.  
- To make them **persistent**, add them to configuration files like `~/.bashrc`, `~/.profile`, or `~/.bash_profile`.  
- Example:  
  `echo "export APP_ENV=production" >> ~/.bashrc`  
  `source ~/.bashrc`  
- Now every new session loads the variable automatically.  

## 4. Processes in Linux

### 4.1 Understanding Processes
- A **process** is a running instance of a program.  
- **User processes**: Started by a logged-in user.  
- **System processes**: Background services for the OS (e.g., networking).  

### 4.2 Viewing Processes
- List all processes: `ps aux`  
- Interactive viewer: `top`  
- Improved viewer (if installed): `htop`  

### 4.3 Running Processes in Background
- Run a program in the background: `python3 script.py &`  
- Bring it back to foreground: `fg`  

### 4.4 Stopping Processes
- Find Process ID (PID): `ps aux | grep script.py`  
- Kill process by PID: `kill 1234`  
- **Kill signals:**  
  - `SIGTERM` (15): Graceful shutdown.  
  - `SIGKILL` (9): Force kill.  
  - `SIGSTOP`: Pause without killing.  

## 5. File Permissions

### 5.1 Basics
- Linux permissions apply to:  
  - **User (u)** → owner.  
  - **Group (g)** → same group users.  
  - **Others (o)** → everyone else.  
- Permissions:  
  - **Read (r)** → view.  
  - **Write (w)** → modify/delete.  
  - **Execute (x)** → run as program.  

### 5.2 Viewing Permissions
- `ls -l`  
- Example: `-rwxr-xr--`  
  - User = rwx (read, write, execute)  
  - Group = r-x (read, execute)  
  - Others = r-- (read only)  

### 5.3 Modifying Permissions
- Symbolic mode:  
  - `chmod u+x script.sh` → add execute for user.  
  - `chmod g-w file.txt` → remove write for group.  
  - `chmod o+r report.txt` → add read for others.  
- Numeric mode:  
  - `r = 4`, `w = 2`, `x = 1`  
  - Example: `chmod 755 script.sh`  
    - User = 7 (rwx)  
    - Group = 5 (r-x)  
    - Others = 5 (r-x)  

## 6. AWS EC2 Instance Lifecycle
- **Running** → Active and consuming resources.  
- **Stopped** → Shut down but can be restarted.  
- **Terminated** → Permanently deleted.  

### Best Practice for Sandbox Work
- At the end of a session:  
  - **Terminate** instances to avoid costs.  
  - Since they are temporary, new ones can be created quickly.  
- Example connection:  
  `ssh -i my-key.pem ubuntu@ec2-3-120-45-67.compute-1.amazonaws.com`  

## Key Takeaways
- Linux is the backbone of most cloud environments.  
- Core Linux skills for cloud work include:  
  - Navigating directories (`/root`, `/home`).  
  - Using `sudo` for superuser tasks.  
  - Moving files with `mv`.  
  - Managing **environment variables** and making them persistent.  
  - Monitoring and controlling **processes**.  
  - Setting and understanding **permissions** (symbolic & numeric).  
- AWS EC2 instances are lightweight Linux servers:  
  - Accessed via **SSH**.  
  - Should be **terminated when not needed**.  

---



# Bash Scripting, Debugging, and File Transfer in AWS EC2

## 1. Introduction

When working with **AWS EC2 Linux instances**, tasks are often repeated: updating packages, installing software, configuring services.

* Running these commands **manually** is fine at first, but it becomes repetitive.
* To automate, we use **Bash scripts** — text files containing a sequence of shell commands.
* However, scripts can fail if one command errors out.
* That’s why debugging techniques (like using `echo`) and **testing on fresh instances** are important.

This document explains:

* How Bash scripts work.
* Why and how to use `echo` for debugging.
* Best practices for creating and testing scripts.
* How to copy files from your local machine to an EC2 instance.
* Instance lifecycle management (stop vs terminate).

---

## 2. Bash Scripts

### 2.1 What is a Bash Script?

* A Bash script is simply a **text file** containing Linux commands that you would normally type manually.
* Scripts are executed by the **Bash shell**, which is the default command-line interpreter on most Linux systems (including Ubuntu EC2).

Example of a basic script (`hello.sh`):

```bash
#!/bin/bash
echo "Hello, World!"
```

* The first line `#!/bin/bash` is called a **shebang**. It tells the system to use the Bash interpreter.
* `echo "Hello, World!"` prints a message to the terminal.

### 2.2 Creating and Running a Script

1. Open a new file:

   ```bash
   nano hello.sh
   ```
2. Add script contents (like the example above).
3. Save and exit (`CTRL+O`, `ENTER`, `CTRL+X`).
4. Make it executable:

   ```bash
   chmod +x hello.sh
   ```
5. Run it:

   ```bash
   ./hello.sh
   ```

---

## 3. Using `echo` for Debugging

### 3.1 Why `echo` is Useful

* When a script has many commands, it’s hard to know **which step caused an error**.
* `echo` prints messages to the terminal so you can see:

  * When each step starts.
  * When each step finishes.
  * Which step failed if the script stops.

### 3.2 Example: Script Without `echo`

```bash
#!/bin/bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx -y
sudo systemctl start nginx
```

* If the script fails, you don’t know at which line it stopped.

### 3.3 Example: Script With `echo`

```bash
#!/bin/bash
echo "Starting system update..."
sudo apt update && sudo apt upgrade -y
echo "System update completed."

echo "Installing Nginx..."
sudo apt install nginx -y
echo "Nginx installation completed."

echo "Starting Nginx service..."
sudo systemctl start nginx
echo "Nginx is now running."
```

* If an error occurs, you’ll know exactly which stage failed.

---

## 4. Best Practice: Manual First, Script Later

Luke Fairbrass emphasised: **run commands manually first**.

* Reason:

  * If a script fails, it will stop executing at the error.
  * Debugging inside a script is harder because you don’t get interactive feedback.

### Process:

1. Run commands one by one manually.
2. Confirm they work.
3. Add them into a script with `echo` statements.
4. Test the script on a fresh EC2 instance.

---

## 5. Copying Files from Local to EC2

### 5.1 Why Copy Files?

* You might write your script locally on your laptop (where editing is easier).
* To run it on EC2, you need to transfer the file.

### 5.2 Using `scp` (Secure Copy)

`scp` works like `cp` (copy) but over SSH.

Command structure:

```bash
scp -i <key.pem> <local_file> <remote_user>@<ec2_public_dns>:<remote_path>
```

Example:

```bash
scp -i my-key.pem deploy_nginx.sh ubuntu@ec2-3-120-45-67.compute-1.amazonaws.com:/home/ubuntu/
```

* `-i my-key.pem` → your private key for SSH authentication.
* `deploy_nginx.sh` → file on your local machine.
* `ubuntu@...` → remote EC2 username and address.
* `/home/ubuntu/` → destination directory on EC2.

### 5.3 Alternative: Manual Creation

If `scp` isn’t available:

1. Log into EC2.
2. Create the file with `nano`:

   ```bash
   nano deploy_nginx.sh
   ```
3. Paste the script.
4. Save, exit, make executable (`chmod +x`), and run.

---

## 6. Testing Scripts on Fresh Instances

* A script should work on a **new, clean EC2 instance**.
* Why?

  * Ensures portability (works anywhere).
  * Confirms no hidden dependencies.
  * Validates that your setup is reproducible.

### Process:

1. Terminate your old instance.
2. Create a new EC2 instance.
3. Copy or recreate your script.
4. Run the script from scratch.

If it succeeds → your script is reliable.

---

## 7. EC2 Instance Lifecycle

* **Running** → Active, consuming resources, incurring costs.
* **Stopped** → Instance is shut down but can be restarted later.
* **Terminated** → Permanently deleted, cannot be restarted.

### Best Practice in Training/Sandbox Environments

* **Terminate instances** when finished (not just stop).
* Reason:

  * Prevents charges.
  * Clean slate each time.
  * Easy to create a new instance (less than a minute).

```bash
# Example AWS CLI command to terminate
aws ec2 terminate-instances --instance-ids i-xxxxxxxxxxxxx
```

---

#  Key Takeaways

* **Bash scripts** automate Linux commands.
* Always add a **shebang (`#!/bin/bash`)** at the top.
* Use **`echo` statements** to mark progress and debug errors.
* Run commands manually before scripting them.
* Transfer scripts with **`scp`** or recreate with **`nano`**.
* Test scripts on **fresh EC2 instances** to confirm portability.
* Always **terminate instances** after use in training or testing environments.





