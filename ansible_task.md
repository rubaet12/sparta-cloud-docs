<!-- TOC -->
  * [What is ansible and how it works?](#what-is-ansible-and-how-it-works)
  * [Create a terraform script for controller instance.](#create-a-terraform-script-for-controller-instance)
  * [connecting to your target node from controller:](#connecting-to-your-target-node-from-controller)
  * [Ansible will not use bash command more because it is idempotent it will mess up if we run frequently. So its better to use the module instead.](#ansible-will-not-use-bash-command-more-because-it-is-idempotent-it-will-mess-up-if-we-run-frequently-so-its-better-to-use-the-module-instead)
    * [Ad hoc commands are one-off commands that are executed on the command line of an Ansible control node, without the need for a playbook or any additional configuration.](#ad-hoc-commands-are-one-off-commands-that-are-executed-on-the-command-line-of-an-ansible-control-node-without-the-need-for-a-playbook-or-any-additional-configuration)
  * [How to ping from controller:](#how-to-ping-from-controller)
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

## What is ansible and how it works?
 
* Configuration management tool
* Red hat leads development ,so guarantee of quality open source-code
* written python
* software setting - desired state
* good for application deployment
* operates in any system Linux , windows,mac

## Create a terraform script for controller instance.
 
* Create an  instance by running terraform apply command.
* Once you have created an instance login using ssh command
* run "sudo apt update"
* run "sudo apt upgrade"
* run "sudo apt-add-repository ppa:ansible/ansible"
* run "sudo apt update" again
* run "sudo apt install ansible -y"
* check whether ansible is installed "ansible --version"
* cd into "cd /etc/ansible"
* open a different git bash window and cd into ~.ssh file
* Open a separate Gitbash and then cd into your folder .ssh and tech508-rubaet-aws.pem and then cat or nano to access the private key 
* copy the private key from the "tech508-rubaet-aws.pem"
* Go to the cd /etc/ansible window - nano "tech508-rubaet-aws.pem".
* paste the private key and save
* now when you do ls you can see your private key
* Give only read permission
* "chmod 400 tech508-rubaet-aws.pem"
 
 
## connecting to your target node from controller:
 
* Copy your ssh key from your target node after you login into the EC2 instance.
* Now paste the ssh key in the controller (following from step 31)
* Now you have logged into the targetnode instance
* you check whether it is correct by checking the public ip.
* If you exit you will goback to controller.
 
## Ansible will not use bash command more because it is idempotent it will mess up if we run frequently. So its better to use the module instead.
 
### Ad hoc commands are one-off commands that are executed on the command line of an Ansible control node, without the need for a playbook or any additional configuration.
 
## How to ping from controller:
 
* sudo nano hosts"
* ec2-instance ansible_host=34.245.188.122 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/tech508-rubaet-aws.pem"
* run "ansible all -m ping"
* you can see it ping to the target node.
# Ansible on Ubuntu (AWS) – Easy Setup Guide

This guide is a simplified version of the steps you followed, making it easy to read and repeat. It covers installing Ansible on an Ubuntu 22.04 AWS EC2 controller, connecting with PEM keys, setting up an inventory, and running basic commands.

---

## Step 1: Connect to Your Controller
On your **local machine**:
```bash
cd ~/.ssh
chmod 400 "tech508-rubaet-aws.pem"
ssh -i "tech508-rubaet-aws.pem" ubuntu@<controller_public_ip>
```

On the **controller**:

```bash
sudo apt update && sudo apt upgrade -y
uname -a
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

Edit the inventory file:

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

Successful output:

```text
ec2-app-instance | SUCCESS => {
  "ping": "pong"
}
```

If you get `Permission denied (publickey)`:

* Check the SSH user (`ubuntu`, `ec2-user`, etc.)
* Confirm PEM file permissions with `chmod 400`
* Make sure your AWS Security Group allows port 22 (SSH)

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

Playbook to install and run Nginx:

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

Save it as `site.yml` and run:

```bash
ansible-playbook site.yml
```

---

## Step 7: Troubleshooting

* **Permission denied** → Wrong SSH user, key permissions, or Security Group.  
* **Hosts list empty** → Inventory not set up.  
* **Timeout** → Public IP or firewall issue.  
* **Python warning** → Add `ansible_python_interpreter=/usr/bin/python3` in inventory.  

---

## Quick Commands

```bash
# Ping all
ansible all -m ping

# Ping a group
ansible web -m ping

# Install a package
ansible web -b -m apt -a "name=htop state=present update_cache=yes"

# Copy a file
ansible web -m copy -a "src=/etc/hosts dest=/tmp/hosts mode=0644"

# Run a playbook
ansible-playbook -i /etc/ansible/hosts site.yml
```

---

## Done 

You now have:

1. Ansible installed on the controller  
2. SSH key with correct permissions  
3. Inventory configured  
4. Successful Ansible commands running on your EC2 instance  
