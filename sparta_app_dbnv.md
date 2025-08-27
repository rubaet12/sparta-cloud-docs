```bash
#!/bin/bash

# MongoDB Provisioning Script
# This script:
#  Updates the system
#  Installs MongoDB 7.0 (specific version)
#  Imports the MongoDB GPG key
#  Adds the MongoDB APT repository
#  Configures MongoDB to allow external connections
#  Starts and enables the MongoDB service

#  Update the package list 
echo "update..."
sudo DEBIAN_FRONTEND=noninteractive apt-get update
echo "update done"
echo

#  Upgrade all installed packages 
echo "upgrade..."
sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y
echo "upgrade done"
echo

#  Install required tools: gnupg and curl 
sudo DEBIAN_FRONTEND=noninteractive apt-get install gnupg curl

# Import MongoDB public GPG key (for verifying packages) 
echo "import public key"
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
gpg --dearmor | sudo tee /usr/share/keyrings/mongodb-server-7.0.gpg > /dev/null
echo "imported public key"
echo

# Create APT source list for MongoDB 7.0 
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

# Update package list again to include MongoDB repo 
sudo DEBIAN_FRONTEND=noninteractive apt-get update

# Install MongoDB 7.0 and related tools (specific versions) 
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
echo "Configuring MongoDB to Allow External Connections"
echo

#  Backup the current MongoDB config file 
sudo cp /etc/mongod.conf /etc/mongod.conf.bk

#  Update bindIp from 127.0.0.1 to 0.0.0.0 to allow external connections 
sudo sed -i 's/bindIp: 127.0.0.1/bindIp: 0.0.0.0/' /etc/mongod.conf
echo "bindIp updated to 0.0.0.0"
echo

#  Start and enable MongoDB service 
echo
echo "Starting MongoDB..."
echo
sudo systemctl start mongod
sudo systemctl enable mongod

echo
echo "MongoDB provisioned and running"
echo
```
