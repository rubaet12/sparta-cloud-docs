<!-- TOC -->
  * [What is Ansible and how it works?](#what-is-ansible-and-how-it-works)
  * [Create a Terraform script for controller instance](#create-a-terraform-script-for-controller-instance)
  * [Connecting to your target node from controller](#connecting-to-your-target-node-from-controller)
  * [Why not use bash commands directly in Ansible?](#why-not-use-bash-commands-directly-in-ansible)
    * [Ad hoc commands](#ad-hoc-commands)
  * [How to ping from controller](#how-to-ping-from-controller)
* [Ansible on Ubuntu (AWS) – Easy Setup Guide](#ansible-on-ubuntu-aws--easy-setup-guide)
  * [Step 1: Connect to Your Controller](#step-1-connect-to-your-controller)
  * [Step 2: Install Ansible](#step-2-install-ansible)
  * [Step 3: Add Hosts to Inventory](#step-3-add-hosts-to-inventory)
  * [Step 4: Test the Connection](#step-4-test-the-connection)
  * [Step 5: Run Commands](#step-5-run-commands)
  * [Step 6: Create a Playbook](#step-6-create-a-playbook)
  * [Step 7: Troubleshooting](#step-7-troubleshooting)
  * [Quick Commands](#quick-commands)
  * [Done](#done-)
<!-- TOC -->
---

## What is Ansible and how it works?

* **Ansible** is an open-source automation and configuration management tool (maintained by Red Hat).  
* It is **written in Python** and uses **modules** to define how systems should be set up.  
* **Agentless**: you don’t need to install any software on target machines. Ansible connects over **SSH (Linux/macOS)** or **WinRM (Windows)**.  
* It is mainly used for:  
  - Setting up servers  
  - Deploying applications  
  - Automating repetitive tasks  
  - Managing multiple servers at once  
* Works across **Linux, Windows, and macOS**.  

---

## Create a Terraform script for controller instance

Terraform helps create infrastructure automatically. Here’s a simple example to launch a controller EC2 instance:

```hcl
provider "aws" {
  region = "eu-west-1"
}

resource "aws_instance" "controller" {
  ami           = "ami-0c02fb55956c7d316" # Ubuntu 22.04 LTS in eu-west-1
  instance_type = "t2.micro"
  key_name      = "tech508-rubaet-aws"

  tags = {
    Name = "Ansible-Controller"
  }
}
```

Steps:

1. Save this file as `main.tf`.
2. Run:

   ```bash
   terraform init
   terraform apply -auto-approve
   ```
3. Connect to your instance:

   ```bash
   ssh -i tech508-rubaet-aws.pem ubuntu@<controller_public_ip>
   ```

On the controller:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt-add-repository ppa:ansible/ansible -y
sudo apt update
sudo apt install ansible -y
ansible --version
```

Prepare your PEM file:

```bash
cd /etc/ansible
sudo nano tech508-rubaet-aws.pem   # paste your private key here
chmod 400 tech508-rubaet-aws.pem
ls   # check file is saved
```

---

## Connecting to your target node from controller

* Log in to your **target EC2 instance** once to get the SSH key or details.
* Paste the target node’s SSH key into the controller.
* Verify the connection using the **target’s public IP**.
* If you exit, you’ll return to the controller.

---

## Why not use bash commands directly in Ansible?

* Ansible is **idempotent** → running the same playbook multiple times won’t break anything.
* If you use raw **bash commands**, repeating them can cause errors or duplicates.
* It’s better to use **Ansible modules** (like `apt`, `copy`, `service`) which are safer and repeatable.

### Ad hoc commands

**Simple definition**:
*In Ansible, an ad hoc command is a single, one-time command that you run directly from the control node’s command line to perform a quick task on managed nodes, without writing a playbook*

Examples:

```bash
ansible all -m ping          # test connectivity
ansible web -a "uptime"      # run 'uptime' on web group
```

Use them for **testing or small tasks**, not for big setups.

---

## How to ping from controller

1. Edit your Ansible hosts file:

   ```bash
   sudo nano /etc/ansible/hosts
   ```
2. Add your target node:

   ```ini
   ec2-instance ansible_host=34.245.188.122 ansible_user=ubuntu \
     ansible_ssh_private_key_file=~/.ssh/tech508-rubaet-aws.pem
   ```
3. Run ping test:

   ```bash
   ansible all -m ping
   ```
4. Expected result:

   ```text
   ec2-instance | SUCCESS => {
     "ping": "pong"
   }
   ```

---

# Ansible on Ubuntu (AWS) – Easy Setup Guide

This guide shows how to install Ansible on an Ubuntu AWS controller, set up SSH keys, configure the inventory, and run commands/playbooks.

---

## Step 1: Connect to Your Controller

On your **local machine**:

```bash
cd ~/.ssh
chmod 400 tech508-rubaet-aws.pem
ssh -i "tech508-rubaet-aws.pem" ubuntu@<controller_public_ip>
```

On the **controller**:

```bash
sudo apt update && sudo apt upgrade -y
uname -a   # check system
```

---

## Step 2: Install Ansible

```bash
sudo apt-add-repository ppa:ansible/ansible -y
sudo apt update
sudo apt install -y ansible
ansible --version
```

---

## Step 3: Add Hosts to Inventory

```bash
sudo nano /etc/ansible/hosts
```

Example setup:

```ini
[web]
ec2-app-instance ansible_host=52.208.211.83 ansible_user=ubuntu \
  ansible_ssh_private_key_file=/home/ubuntu/.ssh/tech508-rubaet-aws.pem

[controller]
localhost ansible_connection=local
```

---

## Step 4: Test the Connection

```bash
cd /etc/ansible
ansible all -m ping
ansible web -m ping
```

Expected output:

```text
ec2-app-instance | SUCCESS => {
  "ping": "pong"
}
```

If you see **Permission denied**:

* Check the SSH user (`ubuntu`, `ec2-user`, etc.).
* Ensure PEM file has `chmod 400`.
* Check AWS Security Group for port **22**.

---

## Step 5: Run Commands

```bash
ansible web -a "uname -a"
```

Example result:

```text
Linux ip-10-0-1-28 6.8.0-1033-aws x86_64 GNU/Linux
```

---

## Step 6: Create a Playbook

Example playbook to install and start Nginx:

```yaml
---
- name: Configure web servers
  hosts: web
  become: yes
  tasks:
    - name: Update apt cache
      ansible.builtin.apt:
        update_cache: yes

    - name: Install nginx
      ansible.builtin.apt:
        name: nginx
        state: present

    - name: Ensure nginx is running
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: yes
```

Save as `site.yml` and run:

```bash
ansible-playbook site.yml
```

---

## Step 7: Troubleshooting

* **Permission denied** → Wrong SSH user, bad key, or SG issue.
* **Hosts list empty** → Inventory missing or not configured.
* **Timeout** → Wrong public IP or firewall blocking.
* **Python warning** → Add `ansible_python_interpreter=/usr/bin/python3` in inventory.

---

## Quick Commands

```bash
# Ping all hosts
ansible all -m ping

# Ping a group
ansible web -m ping

# Install a package (htop)
ansible web -b -m apt -a "name=htop state=present update_cache=yes"

# Copy a file
ansible web -m copy -a "src=/etc/hosts dest=/tmp/hosts mode=0644"

# Run a playbook
ansible-playbook -i /etc/ansible/hosts site.yml
```

---
````
## Done 

At this point you have:

1. Ansible installed on the controller
2. SSH key permissions correctly set
3. Target hosts added to inventory
4. Successful Ansible commands running against your EC2 instance

````

