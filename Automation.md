### AWS EC2 & Auto Scaling Group Setup

#### 1. Launch EC2 Instance
- **Login to AWS**
- In **EC2**, click **Launch Instance** and create a new one:
  - **Name**: `tech508-rubaet-for-asg-app-lt`
  - **AMI**: Go to **Owned by me** → select `tech508-rubaet-test-sparta-app-ready-to-run`
  - **Instance type**: `t3.micro`
  - **Key pair**: (Select your key pair)
  - **Security group**: `tech508-rubaet-sparta-app-allow-SSH-HTTP-3000`
- **Advanced settings** → add user data:
  ```bash
  #!/bin/bash
  cd repo/app
  pm2 start app.js
  ```
- **Launch Template**:
  - Save as a launch template
  - **Actions → Launch instance with template**
- **Launch Instance**
- Copy the **IP Address** and paste in your browser to test.

---

#### 2. Create Auto Scaling Group (ASG)
- Go to **Auto Scaling Groups** (bottom left)
- Click **Create Auto Scaling Group**:
  - **Name**: `tech508-rubaet-app-asg`
  - **Launch template**: `tech508-rubaet-app-asg-lt`
- **Next**
- **Network**: Select subnets with `DevOPpsStudentDefault` in **1a**, **1b**, and **1c**
- **Next**
- Attach to a **new load balancer**:
  - **Load balancer name**: `tech508-rubaet-app-asg-lb`
  - **Scheme**: `internet-facing`
- **Listeners & Routing**:
  - Default routing → Create new target group: `tech508-rubaet-app-asg-lb-tg`
- **Additional health checks**:
  - Turn on **Elastic Load Balancing health checks**
  - **Health check grace period**: `180 seconds`
- **Next**
- **Group size**:
  - Desired capacity: `2`
  - Minimum: `2`
  - Maximum: `3`
- **Scaling policy**:
  - Target tracking scaling policy (automatic scaling)
  - **Instance warmup**: `180 seconds`
- **Next** → **Next**
- **Tags**:
  - Key: `Name`
  - Value: `tech508-rubaet-app-asg-HA-SC`
- **Create Auto Scaling Group**

---

#### 3. Test Load Balancer
- Click your ASG name
- In **Load Balancers** (bottom left), search for `tech508-rubaet-app-asg-lb`
- Find the **DNS name** of the load balancer
- Copy and paste the DNS into your browser
