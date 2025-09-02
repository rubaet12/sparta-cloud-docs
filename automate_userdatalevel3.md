
<!-- TOC -->
* [Automate User Data Level 3](#automate-user-data-level-3)
  * [1. Launch a New App Database Instance](#1-launch-a-new-app-database-instance)
  * [Connect to DB Instance](#connect-to-db-instance)
  * [2. Launch a New App Instance](#2-launch-a-new-app-instance)
  * [Connect to App Instance](#connect-to-app-instance)
  * [3. Documentation for AMI Creation](#3-documentation-for-ami-creation)
  * [Create DB AMI](#create-db-ami)
  * [Create App AMI](#create-app-ami)
  * [4. Launch Instances from AMIs](#4-launch-instances-from-amis)
  * [Database Image](#database-image)
  * [App Image](#app-image)
  * [Advanced Settings - User Data Script](#advanced-settings---user-data-script)
<!-- TOC -->
# Automate User Data Level 3

## 1. Launch a New App Database Instance
- **Name**: `tech508-rubaet-sparta-app-db`
- **AMI**: Ubuntu 22.04 LTS
- **Key Pair**: Use your designated key pair
- **Security Group**: `tech508-rubaet-sparta-app-db-SSH`
  - **Inbound Rules**:
    - SSH (22) from `0.0.0.0/0`
    - Custom TCP (Port Range: 27017, Description: SPARTA APP) from `0.0.0.0/0`
- **Advanced Settings**:
  - Scroll to bottom and paste `prov-db.sh` script
- **Launch the Instance**
````bash
# MongoDB Provisioning Script
# This script:
#  Updates the system
#  Installs MongoDB 7.0 (specific version)
#  Imports the MongoDB GPG key
#  Adds the MongoDB APT repository
#  Configures MongoDB to allow external connections
#  Starts and enables the MongoDB service


#!/bin/bash
 
#Scipt for the database
 
# Update the package list to make sure the latest versions are installed
echo "update..."
sudo DEBIAN_FRONTEND=noninteractive apt-get update
echo "update done"
echo
 
echo "upgrade..."
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo "upgrade done"
echo
 
sudo DEBIAN_FRONTEND=noninteractive apt-get install gnupg curl
 
echo "import public key"
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
gpg --dearmor | sudo tee /usr/share/keyrings/mongodb-server-7.0.gpg > /dev/null
echo "imported public key"
echo
 
#create list file
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
 
sudo DEBIAN_FRONTEND=noninteractive apt-get update
 
#install mongodb
sudo DEBIAN_FRONTEND=noninteractive apt-get install -y \
   mongodb-org=7.0.22 \
   mongodb-org-database=7.0.22 \
   mongodb-org-server=7.0.22 \
   mongodb-mongosh \
   mongodb-org-shell=7.0.22 \
   mongodb-org-mongos=7.0.22 \
   mongodb-org-tools=7.0.22 \
   mongodb-org-database-tools-extra=7.0.22
 
   echo
echo " Configuring MongoDB to Allow External Connections"
echo
 
# Backup existing config
sudo cp /etc/mongod.conf /etc/mongod.conf.bk
 
# Update bindIp to 0.0.0.0
sudo sed -i 's/bindIp: 127.0.0.1/bindIp: 0.0.0.0/' /etc/mongod.conf
echo "bindIp updated to 0.0.0.0"
echo
 
echo
echo " Starting MongoDB..."
echo
sudo systemctl start mongod
sudo systemctl enable mongod
 
echo
echo
echo " MongoDB provisioned and running "
echo

---
````
## 2. Launch a New App Instance
- **Name**: `tech508-rubaet-test-sparta-app`
- **AMI**: Ubuntu 22.04 LTS
- **Key Pair**: Use your designated key pair
- **Security Group**: `tech508-rubaet-sparta-app-SSH`
  - **Inbound Rules**:
    - SSH (22) from `0.0.0.0/0`
    - HTTP (80) from `0.0.0.0/0`
    - Custom TCP (Port Range: 3000, Description: SPARTA APP) from `0.0.0.0/0`
- **Advanced Settings**:
  - Scroll to bottom and paste `prov-app.sh` script
  - **IMPORTANT**: Paste the **private IP** from the **DB instance** into the `export DB_HOST` section of the script
- **Launch the Instance**

## Connect to App Instance
```bash
# Open Git Bash terminal
cd .ssh
chmod 400 "tech508-rubaet-aws.pem"
ssh -i "tech508-rubaet-aws.pem" ubuntu@ec2-34-244-208-198.eu-west-1.compute.amazonaws.com
sudo systemctl status nginx  # Check if NGINX is running
```
**Screenshot of the output in the terminal to check if NGINX is running which is**

![img_7.png](Images/img_7.png)

- Copy **public IP** and paste it into browser (start with `http://`)
- Append `/posts` to the URL to check functionality

---

**front page**

![img_5.png](Images/img_5.png)

**posts page**

![img_6.png](Images/img_6.png)

## 3. Documentation for AMI Creation

## Create DB AMI
1. Go to **Instances > Actions > Image and Templates > Create Image**
2. Name: `tech508-rubaet-test-sparta-app-ready-to-run-database`
3. Add Tag:
   - **Key**: `Name`
   - **Value**: `tech508-rubaet-test-sparta-app-ready-to-run-database`
4. Click **Create Image**

## Create App AMI
1. Go to **Instances > Actions > Image and Templates > Create Image**
2. Name: `tech508-rubaet-test-sparta-app-ready-to-run-app`
3. click on "Tag image and snapshots separately"
3. Add Tag:
   - **Key**: `Name`
   - **Value**: `tech508-rubaet-test-sparta-app-ready-to-run-app`
4. Click **Create Image**

---

## 4. Launch Instances from AMIs

## Database Image
1. Go to **Images > AMIs**
2. Select **database AMI** and launch an instance
3. **Name**: `tech508-rubaet-test-sparta-app-db-image`
4. **Security Group**: `tech508-rubaet-sparta-app-db-Image-SSH`
   - **Inbound Rules**:
     - SSH (22) from `0.0.0.0/0`
     - Custom TCP (27017, Description: SPARTA APP) from `0.0.0.0/0`
     - Click launch instance 

## App Image
1. Select **app AMI** and launch an instance
2. **Name**: `tech508-rubaet-test-sparta-app-image`
3. **Key Pair**: Use your designated key pair
4. **Security Group**: `tech508-rubaet-sparta-app-SSH`
   - **Inbound Rules**:
     - SSH (22) from `0.0.0.0/0`
     - HTTP (80) from `0.0.0.0/0`
     - Custom TCP (3000, Description: SPARTA APP) from `0.0.0.0/0`

## Advanced Settings - User Data Script
```bash
#!/bin/bash
export DB_HOST=mongodb://172.31.21.64:27017/posts  # Replace with private IP of DB instance (ami one)
cd repo/app
pm2 start app.js
```
   - After click launch instance 
**Output**

![img_9.png](Images/img_9.png)

![img_8.png](Images/img_8.png)
- **IMPORTANT**: Make sure to update `DB_HOST` with the actual **private IP** of the **DB instance**
- Copy **public IP** and check in browser using `http://<public-ip>/posts`
