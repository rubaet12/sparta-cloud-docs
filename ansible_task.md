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
