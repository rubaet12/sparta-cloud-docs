# Setting Up an SSH Key for GitHub

This guide explains how to create and configure an SSH key to securely connect GitHub with your local machine.

---

### **Step 1: Generate an SSH Key Pair**

1.  Open **Git Bash**.
2.  Type `pwd` to check your current directory. This should be your home directory (`~`).
3.  Type `ls -a` to list all files and folders, including hidden ones. If you see a folder named `.ssh`, you already have one.
4.  If you don't have one, create it by typing `mkdir .ssh`.
5.  Move into the `.ssh` folder with `cd .ssh`. Type `pwd` again; you should see a path like `/c/Users/your_name/.ssh`.
6.  Generate a new SSH key by running the following command:
    ```bash
    ssh-keygen -t rsa -b 4096 -C "youremail@spartaglobal.com"
    ```
    * `ssh-keygen`: The command to create a new SSH key.
    * `-t rsa`: Specifies the RSA encryption type.
    * `-b 4096`: Sets the key length to 4096 bits for better security.
    * `-C`: Adds a comment to the key, typically your email, to help you identify it.

---

### **Step 2: Add the Key to GitHub**

1.  Log in to your GitHub account.
2.  Navigate to **Settings → SSH and GPG keys**.
3.  Click **New SSH key**.
4.  Provide a descriptive title for your key (e.g., `MyLaptopKey`).
5.  Return to Git Bash and type `cat id_rsa.pub`. This command will display your public key as a long string.
6.  Copy the entire public key string and paste it into the **Key** box on GitHub.
7.  Click **Add SSH key**.

---

### **Step 3: Start the SSH Agent**

1.  In Git Bash, run `eval "$(ssh-agent -s)"`. This starts the SSH agent in the background.
2.  Add your SSH key to the agent by typing `ssh-add ~/.ssh/id_rsa`.

---

### **Step 4: Test Your Connection**

1.  Type `ssh -T git@github.com`.
2.  When prompted to continue, type `yes`.
3.  If the connection is successful, you will see a message like:
    ```
    Hi username! You've successfully authenticated, but GitHub does not provide shell access.
    ```