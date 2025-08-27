### The manual to setting up the sparta app via AWS

# Task I: Setting Up a Full Web App Using Two AWS EC2 Instances (Frontend + Backend)

This guide walks you through how to get a simple full-stack app running. The app has two parts:
1. A **frontend** – what users see when they go to a website
2. A **backend/database** – where the app stores and retrieves data

You will set up **two virtual machines (VMs)** using AWS EC2:
- One for the **Sparta frontend app**
- One for the **MongoDB backend database**

---

## Part 1: Set Up the Frontend Application (Sparta App)

### Step 1: Launch AWS EC2 Instance for Frontend

Go to [AWS EC2](https://console.aws.amazon.com/ec2) and click **Launch Instance**.

Set the following options:
- **Name:** `tech508-name-test-sparta-app`
- **AMI (Image):** Ubuntu Server 24.04 LTS
- **Key Pair:** Select `tech508_rubaet-aws`
- **Network Settings:** Create a new security group:
  - **Name:** `tech508-name-sparta-app-allow-SSH-HTTP-3000`
  - Allow:
    - **SSH** on port **22** (for connecting)
    - **HTTP** on port **80** (for website access)
    - **Custom TCP** on port **3000** (to run the app)

Click **Launch Instance**.

---

### Step 2: Connect to the Frontend VM Using Git Bash

1. In the AWS EC2 console, click your instance name, then click **Connect**.
2. Copy the **SSH command** (it looks like `ssh -i "key.pem" ubuntu@...`).
3. Open **Git Bash** on your computer.
4. Run:
   ```bash
   cd ~/.ssh
   ```
5. Paste and run the **SSH command** you copied. Type `yes` if prompted.

You are now connected to your remote Ubuntu server!

---

### Step 3: Install Required Software on Frontend VM

Now install the necessary tools and run your web app.

Copy and paste the following commands one at a time:

```bash
# Update the system
sudo apt-get update
sudo apt-get upgrade -y

# Install web server software
sudo apt-get install nginx -y

# Install Node.js (used to run the app)
sudo DEBIAN_FRONTEND=noninteractive bash -c "curl -fsSL https://deb.nodesource.com/setup_20.x | bash -" && \
sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs
```

---

### Step 4: Download and Run the App

Run the following commands:

```bash
# Download the app code from GitHub
git clone https://github.com/Abz299/tech508-spartaapp.git repo

# Go into the app folder
cd repo/tech508-spartaapp

# Install required packages
npm install

# Start the app
npm start
```

If successful, your app is running on port `3000`.

---

### Step 5: Check If the App Works

1. Go back to AWS and copy the **Public IPv4 address** of your app instance.
2. Paste it into your browser like this:
   - `http://your-ip` → You’ll see an Nginx welcome page.
   - `http://your-ip:3000` → You’ll see the Sparta app.
   - `http://your-ip:3000/getposts` → Will NOT work yet because we haven’t set up the database.

---

## Part 2: Set Up the Backend Database (MongoDB)

### Step 6: Launch AWS EC2 Instance for MongoDB

Repeat the process from Step 1, but this time:

- **Name:** `tech508-name-test-sparta-app-db`
- **AMI (Image):** Ubuntu Server 22.04 LTS
- **Key Pair:** Select `tech508_rubaet-aws`
- **Network Settings:** Create a new security group:
  - **Name:** `tech508-name-sparta-app-db-allow-MONGODB`
  - Allow:
    - **SSH** on port **22**
    - **Custom TCP** on port **27017** (MongoDB default port)

Click **Launch Instance**.

---

### Step 7: Connect to MongoDB VM Using Git Bash

Same process as before:

1. In the AWS EC2 console, click **Connect** on your database instance.
2. Copy the **SSH command**.
3. In Git Bash:
   ```bash
   cd ~/.ssh
   ```
4. Paste and run the command.

---

### Step 8: Install MongoDB on the Database VM

Now we install the MongoDB software:

```bash
# Update the system
sudo apt-get update
sudo apt-get upgrade -y

# Install MongoDB tools
sudo apt-get install gnupg curl

# Add MongoDB repository to your system
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/8.0 multiverse" | \
sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list

# Update again to include MongoDB
sudo apt-get update

# Install MongoDB 8.0
sudo apt-get install -y \
mongodb-org=8.0.12 \
mongodb-org-database=8.0.12 \
mongodb-org-server=8.0.12 \
mongodb-mongosh \
mongodb-org-shell=8.0.12 \
mongodb-org-mongos=8.0.12 \
mongodb-org-tools=8.0.12 \
mongodb-org-database-tools-extra=8.0.12
```

---

### Step 9: Allow Remote Access to MongoDB

MongoDB only allows local access by default. We need to change this:

```bash
# Backup config file first
sudo cp /etc/mongod.conf /etc/mongod.conf.bk

# Change bind IP so MongoDB can be accessed from other machines
sudo sed -i 's/bindIp: 127.0.0.1/bindIp: 0.0.0.0/' /etc/mongod.conf
```

Now start MongoDB and enable it to start on boot:

```bash
sudo systemctl enable mongod
sudo systemctl start mongod
sudo systemctl status mongod  # Should say "active (running)"
```

---

## Part 3: Link Frontend with Backend

### Step 10: Connect the App to the MongoDB Database

Now go **back to the Sparta App terminal** (Frontend VM) and do the following:

1. Stop the app:
   ```bash
   Ctrl + C
   ```

2. Set the MongoDB database address. Replace `<PRIVATE DB IP>` with your database VM’s **private IPv4 address** from AWS:

   ```bash
   export DB_HOST="mongodb://<PRIVATE DB IP>:27017/posts"
   ```

   Example:
   ```bash
   export DB_HOST="mongodb://172.31.30.231:27017/posts"
   ```

3. Check that it’s saved:
   ```bash
   printenv DB_HOST
   ```

4. Restart the app:
   ```bash
   npm install
   npm start
   ```

---

## Final Check

- Open `http://<APP PUBLIC IP>:3000` → You should see the Sparta app.
- Open `http://<APP PUBLIC IP>:3000/getposts` → Now it should work and fetch data from the database.

Congratulations! Your full app is now live and connected to a remote MongoDB database.


### App script 
```bash
#!/bin/bash

# Provisioning Script
# This script sets up a Node.js app with MongoDB and Nginx reverse proxy.
# It:
# Updates and upgrades the system
# Installs Nginx and Node.js
# Clones the app from GitHub
# Installs dependencies
# Starts the app
# Configures Nginx as a reverse proxy so users don’t need to type :3000

#  Update the package list 
echo "update..."
sudo DEBIAN_FRONTEND=noninteractive apt-get update
echo "update done"
echo

# Upgrade all installed packages 
echo "upgrade..."
sudo DEBIAN_FRONTEND=noninteractive apt-get upgrade -y
echo "upgrade done"
echo

#  Install Nginx web server 
echo "install nginx..."
sudo DEBIAN_FRONTEND=noninteractive apt install nginx -y
echo "nginx install complete"
echo

#  Install Node.js (version 20.x) 
echo "install node.js..."
sudo DEBIAN_FRONTEND=noninteractive bash -c "curl -fsSL https://deb.nodesource.com/setup_20.x | bash -" && \
sudo DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs
echo "node.js install complete"
echo

#  Clone the app repository from GitHub 
echo "cloning git repo..."
git clone https://github.com/rubaet12/Tech508-sparta-app.git repo
cd repo
echo "git cloning complete"
echo

#  Install Node.js dependencies listed in package.json 
echo "installing npm dependencies..."
npm install
echo "npm install complete"
echo

#  Set environment variable for MongoDB connection 
export DB_HOST=mongodb://172.31.31.106:27017/posts
echo "db_host is set"
echo

#  Free up port 3000 if another process is using it 
echo "Checking if anything is already using port 3000..."
PID=$(sudo lsof -t -i:3000 || true)  # Finds process ID using port 3000
if [ -n "$PID" ]; then
  echo "Port 3000 is in use by PID $PID. Killing..."
  sudo kill $PID  # Stops the process so we can use the port
  echo "Port 3000 cleared."
else
  echo "Port 3000 is free."
fi
echo

#  Start the Node.js app in the background 
npm start &  # Runs the app (usually listens on port 3000)

#  Configure Nginx to act as a reverse proxy 
echo "Configuring Nginx reverse proxy..."

# Backup the current Nginx default site config file
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak

# Replace the line that serves static files with a proxy to the Node.js app
# It replaces: try_files $uri $uri/ =404;
# With:        proxy_pass http://localhost:3000;
sudo sed -i 's|try_files.*|proxy_pass http://localhost:3000;|' /etc/nginx/sites-available/default

# Restart Nginx so the changes take effect
sudo systemctl restart nginx
echo "Nginx reverse proxy configured and restarted"
echo
```
### Database script 
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
### How do you run it?
1. Open both your database terminal and app terminal; connected to there respective AWS instances.
2. nano ./app.sh – copy script in with following control s and control x then run chmod +x app.sh
3. nano ./database.sh – copy script in with following control s and control x then run chmod +x database.sh
4. run database via ./database.sh the run app via ./app.sh

# Task V: Implement Reverse Proxy (Access App Without :3000)

## What is a Reverse Proxy?

A reverse proxy lets users visit your web app **without needing to type `:3000`** at the end of the address. For example:

- Without reverse proxy: `http://54.123.45.67:3000`
- With reverse proxy: `http://54.123.45.67`

This is done by setting up **NGINX** to forward all traffic from port `80` (the default web browser port) to port `3000` (where your app runs).

---

## Step-by-Step Instructions

### 1. Add Reverse Proxy Configuration to `app.sh`

Connect to your **Sparta App instance** using Git Bash and open your `app.sh` script:

```bash
nano app.sh
```

Scroll to the very **end of the file** and paste the following code:

```bash
# Set up NGINX Reverse Proxy
sudo sh -c 'cat > /etc/nginx/sites-available/tech508-rubaet-test-sparta-app' <<EOF
server {
    listen 80;
    server_name YOUR_PUBLIC_APP_IP;  # Replace this with your actual public IP address

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade \$http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host \$host;
        proxy_cache_bypass \$http_upgrade;
    }
}
EOF

# Remove any default or old site configs
sudo rm -f /etc/nginx/sites-enabled/default
sudo rm -f /etc/nginx/sites-enabled/tech508-rubaet-test-sparta-app

# Enable the new reverse proxy config
sudo ln -s /etc/nginx/sites-available/tech508-rubaet-test-sparta-app /etc/nginx/sites-enabled/

# Test NGINX configuration for errors
sudo nginx -t

# Restart NGINX to apply the new settings
sudo systemctl restart nginx
```

> Replace `YOUR_PUBLIC_APP_IP` with your **actual app instance public IP** from AWS EC2 (e.g., `3.88.74.100`)

---

### 2. Save and Re-run the Script

After adding the reverse proxy section to `app.sh`:

- Press `Ctrl + S` to save
- Press `Ctrl + X` to exit

Then run your updated script:

```bash
./app.sh
```

This will:
- Run your frontend app
- Configure NGINX as a reverse proxy
- Restart NGINX with the new settings

---

### 3. Test in Your Browser

1. Copy your **public IPv4 address** from your app EC2 instance
2. Paste it into your web browser:
   ```
   http://<your-public-ip>
   ```
3. You should now see the Sparta App **without needing to type `:3000`**

---

### Why This Works

- NGINX listens on port `80`, the default port for web browsers
- NGINX forwards all incoming requests to port `3000` where your app is running
- The user only sees the clean URL, while the internal routing is handled behind the scenes






