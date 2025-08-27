# Terraform AWS EC2 Setup — Complete Script & Documentation
 
## Overview
This document contains a **Terraform configuration** to create an **AWS EC2 instance** with a security group that allows:
- **SSH access (port 22)** only from your public IP
- **Port 3000 access** from anywhere (used for apps like Node.js)
- **HTTP access (port 80)** from anywhere
 
It also includes detailed explanations for each section.
 
---
 
## Full Terraform Script
 
```hcl
# Specify AWS provider and region
provider "aws" {
  region = "eu-west-1" # Ireland region
}
 
# Create a security group to control inbound/outbound traffic
resource "aws_security_group" "allow_ports" {
  name        = "tech508-rubaet-tf-allow-port-22-3000-80"
  description = "Allow SSH from localhost, port 3000 and port 80 from all"
 
  # Inbound Rules
 
  # 1. Allow SSH from your public IP only (replace 127.0.0.1/32 with your actual public IP)
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["127.0.0.1/32"] # Replace with your real public IP
  }
 
  # 2. Allow port 3000 from anywhere (for web apps/dev servers)
  ingress {
    from_port   = 3000
    to_port     = 3000
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
 
  # 3. Allow HTTP (port 80) from anywhere
  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
 
  # Outbound Rule
  # Allow all outbound traffic
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
 
# Create EC2 instance
resource "aws_instance" "web" {
  ami                         = "ami-0c1c30571d2dae5c9" # Ubuntu 22.04 LTS AMI ID for eu-west-1
  instance_type               = "t3.micro"
  associate_public_ip_address = true
  key_name                    = "tech508-rubaet-aws" # Replace with your AWS key pair name
  vpc_security_group_ids      = [aws_security_group.allow_ports.id]
 
  tags = {
    Name = "tech508-rubaet-first-vm-terraform"
  }
}
 
# Output EC2 public IP
output "instance_public_ip" {
  description = "Public IP of the EC2 instance"
  value       = aws_instance.web.public_ip
}
```
 
---
 
## Explanation of Each Section
 
### 1. **Provider Configuration**
```hcl
provider "aws" {
  region = "eu-west-1"
}
```
- Specifies the AWS provider.
- Sets the region to **Ireland** (`eu-west-1`).
 
---
 
### 2. **Security Group**
```hcl
resource "aws_security_group" "allow_ports" { ... }
```
- Creates a security group named `tech508-rubaet-tf-allow-port-22-3000-80`.
- **Ingress rules**:
  - Port **22**: SSH access only from your public IP.
  - Port **3000**: Open to all (commonly used for development servers).
  - Port **80**: Open to all (HTTP traffic).
- **Egress rule**:
  - Allows all outbound traffic.
 
---
 
### 3. **EC2 Instance**
```hcl
resource "aws_instance" "web" { ... }
```
- Launches an EC2 instance with:
  - **AMI**: Ubuntu 22.04 LTS (`ami-0c1c30571d2dae5c9`).
  - **Instance type**: `t3.micro` (free-tier eligible).
  - **Public IP**: Automatically assigned.
  - **Security group**: The one we created above.
  - **Key pair**: Replace with your actual AWS key pair for SSH access.
  - **Tags**: Names the instance for identification in AWS.
 
---
 
### 4. **Output**
```hcl
output "instance_public_ip" {
  description = "Public IP of the EC2 instance"
  value       = aws_instance.web.public_ip
}
```
- After deployment, Terraform will display the public IP address of the EC2 instance.
 
---
 
## How to Use
 
1. **Install Terraform**  
   Download and install from: [https://developer.hashicorp.com/terraform/downloads](https://developer.hashicorp.com/terraform/downloads)
 
2. **Authenticate with AWS**  
   Run:
   ```bash
   aws configure
   ```
  Enter your **AWS Access Key ID** and **Secret Access Key** (Never hardcode them in the script).
  Open VS Code from Git Bash (Separate Instruction)

If you’re working in **Git Bash** on Windows and want to open the current directory in **Visual Studio Code**, run:

```bash
code .
```

- `code` is the command-line launcher for VS Code.
- `.` means “open the current folder.”
- Make sure **VS Code is installed** and the command `code` is available in your PATH.
- This is very handy to quickly open your Terraform files or any code in the current folder.


3. **Save the Script**  
   Save the script above as `main.tf`.
 
4. **Initialize Terraform**  
   ```bash
   terraform init
   ```
 
5. **Preview Changes**  
   ```bash
   terraform plan
   ```
 
6. **Apply Changes**  
   ```bash
   terraform apply
   ```
   Type `yes` when prompted.
 
7. **Check Output**  
   Terraform will display the **instance_public_ip** after creation.
 
8. **SSH into the Instance**  
   ```bash
   ssh -i your-key.pem ubuntu@<instance_public_ip>
   ```
 
---
 
## Security Notes
- **Never** commit AWS credentials into Terraform files.
- Restrict SSH access to your actual public IP.
- Terminate instances when not in use to avoid charges.
 

##  Explanation of Each Section

| Section                  | Purpose |
|--------------------------|---------|
| **provider**             | Tells Terraform to use AWS in the `eu-west-1` region. |
| **aws_security_group**   | Defines inbound/outbound traffic rules. |
| **aws_instance**         | Creates the EC2 instance with Ubuntu 22.04 LTS. |
| **output**               | Displays the EC2 public IP after creation. |
| **terraform init**       | Prepares Terraform with the correct plugins. |
| **terraform apply**      | Builds the resources in AWS. |
| **terraform destroy**    | Removes all created resources. |
---