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
 
## How to get date from ansible