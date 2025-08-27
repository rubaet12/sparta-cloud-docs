# Cloud and Virtual Machines with AWS

1: Login to AWS  
2: Go to region and select Europe Ireland for training purposes but it can vary the region depending on your job role or company  
3: On the dashboard you can see loads of services. Go to the top search bar and search EC2 and click on the EC2  
4: EC2 stands for Elastic Compute Cloud  
5: Go on the left-hand side and search for key pairs  
6: Click on it and you will see a bunch of key pairs  
7: Make a key pair first by clicking create key pair (top right)  
8: Give name: devops-upskilling-name-keypair  
9: Click RSA box  
10: Click file format as .pem box  
11: Click create  
12: It gets downloaded (make sure it’s downloaded in the .ssh folder or drag and drop the file into the .ssh folder in your PC)  
13: The file format should be This PC -> Users -> your_name -> .ssh folder  
14: Open Git Bash to check, type `cd .ssh`  
15: Then hit `ls`  
16: Then you should see your file in .pem format  
17: Go back to EC2  
18: Click on instances  
19: Click launch instances  
20: Give a name to your instance, usually something like this: devops-upskilling-name-firstinstance  
21: Configure image option by selecting Ubuntu box  
22: Now select Ubuntu Server 24.04 LTS  
23: Keep scrolling down, don’t touch anything yet  
24: Scroll down until you see instance type (how much hard drive and resources you want to assign). Usually you see t3.micro — leave that, don’t touch it unless you need to assign resources  
25: On the key pair login, select your name. Type your name to see the key pair you created i.e. devops-upskilling-name-keypair  
26: Scroll down to network settings  
27: You need to create a security group if you haven’t already  
28: You do this by clicking create security group  
29: Click allow SSH traffic  
30: Edit network settings  
31: Change launch users on the security group name to something like this: devops-upskilling-name-basic-sg  
32: Description you can copy-paste it so it will be devops-upskilling-name-basic-sg  
33: Inbound security group rules: on type click SSH  
34: Source type: anywhere, IP address value would be 0.0.0.0 which means connect from anywhere  
35: Click on the button add security group  
36: On the type select HTTP  
37: Source type select anywhere  
38: Click on launch instance (bottom right)  
39: Your instance has been successfully launched  
40: You should get a green bar saying success, click on the link ID and it should take you to the new page  
41: Click your name on the instance that is running, top right corner click connect — should take you to a page  
42: Open Git Bash  
43: Type `cd .ssh` to go into your folder  
44: Now open the page with your instance details after pressing the connect button  
45: Copy the top command `chmod 400 "devops-upskilling-name-keypair"` paste it into your Git Bash and hit enter  
46: Copy-paste `ssh -i "devops-upskilling-name-keypair" ubuntu@ec2-34-242-154-28.eu-west-1.compute.amazonaws.com`, paste into Git Bash and hit enter  
47: Say yes and hit enter (avoid using spaces, just type exactly how you see on the screen so it doesn’t bug out)  
48: After pasting, hit enter and you are connected to your virtual machine  
49: To stop your instance go to the top right on instance state and click stop instance to stop  
50: If you want to delete your instance click terminate and it will terminate immediately. It can take a few seconds to show up on your dashboard  
