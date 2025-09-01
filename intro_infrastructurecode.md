<!-- TOC -->
  * [Intro to IaC (Infrastructure as Code)](#intro-to-iac-infrastructure-as-code)
<!-- TOC -->

## Intro to IaC (Infrastructure as Code)

What have we automated?

VMs
* Creation of VMs? No
* Creation of infrastructure where the VMs live? No
* Setup and configure the software to run the app & database - YES

 * Bash Scripting
 * User data
 * Images

What is the problem that needs solving?
* We are still manually "provisioning" servers

By "provisioning" we mean:
* The processing of setting up and configuring the servers

IaC 
* Codifying:
 * what you want (declarative), not the steps (imperative) to do
 * defining the desired state/outcome 

Definition of IaC: A way to manage and provision computers through machine-readable definitions of the infrastructure

Advantages of IaC:
- Keep track of version control
- Very easy to scale
- Easy to keep things consistent and accurate
- Speed and simplicity

The Two main types of IaC Tools:

* Configuration management tools: How to configure the management of software on the system and does it for you by telling it what you want it to do but don't need steps to tell it.
- Examples: Chef, Puppet, Ansible

* Orchestration Tools: Best to use to try define the infrastructure itself as in setting up.
- Examples: Terraform, CloudFormation (AWS), ARM/Bicep Template (Azure), Ansible (can do it but its not designed to)


- tfstate files must be ignored in the .gitignore its a MUST DO as they contain credentials.

.terraform.lock.hcl
* This file locks the terraform provider version

In order:
⦁	terraform init (To initialise it)
⦁	terraform plan (If there are errors it will tell you from this)
⦁	terraform apply (Creates what you want created) & (can create a plan and create it for you if you want to)
⦁	terraform destroy (To get rid of anything created with terraform)
⦁	terraform fmt (can fix the format and run it)


What is Terraform? What is it used for?
⦁	orchestration tool - provisioning infrastructure
⦁	sees infrastructure as immutable (disposable)
⦁	Uses HCL (Hashicorp Language)

Why use Terraform? The benefits?
⦁	easy to use
⦁	sort of open-source, since 2023 uses BSL (Business Source License)
	* some organisations have starting using OpenTofu
⦁	declarative
⦁	Cloud-agnostic (Can use any cloud provider and or multi-cloud support)
⦁	Expressive & Extensible
⦁	Track & detect changes in code

Alternatives to Terraform
⦁	Pulumi
⦁	AWS CloudFormation
⦁	GCP Deployment Manager
⦁	ARM templates for Azure

Who is using Terraform in the industry?
⦁	Loads of industries ALMOST all
In IaC, what is orchestration? 
⦁	Managing lifecycle of infrastructure

How does Terraform act as "orchestrator"?
⦁	creating/deleting/modifying infrastructure resources in the right order

Best practice supplying AWS credentials to Terraform
Uses this order:
1.	through env variable (OKAY TO DO)
2.	Terraform variables in your code (hardcoded) (NEVER DO IT)
3.	AWS CLI shared credentials file (such as aws configure command) (OKAY TO DO)
4.	EC2 instance metadata (assign a role - a set of permissions to EC2 instance) (BEST PRACTICE)

Why use Terraform for different environments? 
⦁	Development / Testing
  * spin up infrastructure when they need it, destroy it when they don't 
⦁	For CI/CD pipelines
⦁	Q&A
⦁	Staging
⦁	Production
  * easily create larger-scale environment 
⦁	Across different environments, there is consistency being used.