# Sparta App Deployment

This guide outlines the steps to deploy the Sparta app, including setting up both the application and a separate database instance.

---

### *Application Instance Setup*

1.  *Launch a new instance:*
    * *Name:* tech508-YOURNAME-test-sparta-app
    * *AMI:* Ubuntu *22.04 LTS*
    * *Key Pair:* Use your designated key pair.
    * *Security Group:* Create a new security group named tech508-YOURNAME-sparta-app.
        * Edit the inbound rules to allow *SSH* and *HTTP* from anywhere (0.0.0.0/0).

2.  *Connect and update:*
    * Launch the instance and click *Connect*.
    * Copy the SSH command.
    * Open Git Bash, navigate to your .ssh folder (cd .ssh), and paste the SSH command, adding -y at the end.
    * Run sudo apt-get update to update the package list.
    * Run sudo apt upgrade -y to upgrade packages.

3.  *Install software:*
    * Install Nginx: sudo apt install nginx -y
    * Install Node.js (version 20.x):
        * sudo DEBIAN_FRONTEND=noninteractive https://deb.nodesource.com/setup_20.x
        * sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs
    * Install npm: sudo apt install npm

4.  *Clone the application:*
    * git clone https://github.com/yourname/tech508-spartaapp.git repo
    * cd repo/tech508-spartaapp

---

### *Database Instance Setup*

1.  *Launch a new instance:*
    * *Name:* tech508-yourname-test-sparta-app-dp
    * *AMI:* Set to *22.04 LTS*.
    * *Security Group:* Create a new security group named tech508-rubaet-sparta-app-db-allow.
        * Configure inbound rules to allow *Custom TCP* on port *27017* from anywhere (0.0.0.0/0).

2.  *Install MongoDB:*
    * sudo apt-get install gnupg curl
    * Import the MongoDB GPG key:
        bash
        curl -fsSL [https://www.mongodb.org/static/pgp/server-7.0.asc](https://www.mongodb.org/static/pgp/server-7.0.asc) | \
        sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
        --dearmor
        
    * Add the MongoDB repository:
        bash
        echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] [https://repo.mongodb.org/apt/ubuntu](https://repo.mongodb.org/apt/ubuntu) jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
        
    * Update the package list: sudo apt-get update
    * Install MongoDB and related tools:
        bash
        sudo apt-get install -y \
        mongodb-org=7.0.22 \
        mongodb-org-database=7.0.22 \
        mongodb-org-server=7.0.22 \
        mongodb-mongosh \
        mongodb-org-shell=7.0.22 \
        mongodb-org-mongos=7.0.22 \
        mongodb-org-tools=7.0.22 \
        mongodb-org-database-tools-extra=7.0.22
        

3.  *Configure and start MongoDB:*
    * cd /etc
    * sudo nano mongod.conf
    * Change the bindIp to 0.0.0.0.
    * Start the MongoDB service: sudo systemctl start mongod

---

### *Final Step*

* Return to your application instance's Git Bash terminal.
* Run the seed script to populate the database: node seeds/seed.js

---

# Sparta App Deployment

This document contains a consolidated script for provisioning both the application and database instances for the Sparta app.

---

### **Provisioning Script (app_and_db_provision.sh)**

This script automates the installation of all necessary software and the cloning of the repository for both the application and the database.

```bash
#!/bin/bash

# ==================================
# Application Instance Provisioning
# ==================================

echo "Starting application instance provisioning..."
echo

echo "Updating package list..."
sudo apt-get update
echo "Update complete."
echo

echo "Upgrading installed packages..."
sudo apt upgrade -y
echo "Upgrade complete."
echo

echo "Installing Nginx..."
sudo apt install nginx -y
echo "Nginx installation complete."
echo

echo "Installing Node.js..."
sudo DEBIAN_FRONTEND=noninteractive bash -c "curl -fsSL [https://deb.nodesource.com/setup_20.x](https://deb.nodesource.com/setup_20.x) | bash -" && \
sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs
echo "Node.js installation complete."
echo

echo "Cloning the git repository..."
git clone [https://github.com/yourname/tech508-spartaapp.git](https://github.com/yourname/tech508-spartaapp.git) repo
echo "Git cloning complete."
echo

echo "Navigating to the application directory and installing npm dependencies..."
cd repo/tech508-spartaapp
npm install
echo "npm install complete."
echo

# Start the application (note: this is a placeholder command, you might need to adjust it)
echo "Starting the application..."
# The 'start npm' command is not a standard shell command. 
# You would likely use 'npm start' or similar, depending on your package.json.
# For demonstration purposes, we will use 'npm start'
npm start
echo "Application started."
echo

# ==================================
# Database Instance Provisioning
# ==================================

echo "Starting database instance provisioning..."
echo

echo "Updating package list..."
sudo apt-get update
echo "Update complete."
echo

echo "Upgrading installed packages..."
sudo apt upgrade -y
echo "Upgrade complete."
echo

echo "Installing gnupg and curl..."
sudo apt-get install -y gnupg curl
echo "gnupg and curl installed."
echo

echo "Importing MongoDB public key..."
curl -fsSL [https://www.mongodb.org/static/pgp/server-7.0.asc](https://www.mongodb.org/static/pgp/server-7.0.asc) | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
echo "MongoDB public key imported."
echo

echo "Creating MongoDB list file..."
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] [https://repo.mongodb.org/apt/ubuntu](https://repo.mongodb.org/apt/ubuntu) jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
echo "List file created."
echo

echo "Updating package list after adding MongoDB repository..."
sudo apt-get update
echo "Update complete."
echo

echo "Installing MongoDB..."
sudo apt-get install -y \
   mongodb-org=7.0.22 \
   mongodb-org-database=7.0.22 \
   mongodb-org-server=7.0.22 \
   mongodb-mongosh \
   mongodb-org-shell=7.0.22 \
   mongodb-org-mongos=7.0.22 \
   mongodb-org-tools=7.0.22 \
   mongodb-org-database-tools-extra=7.0.22
echo "MongoDB installation complete."
echo

echo "Provisioning script finished."