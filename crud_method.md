

# S3 CRUD Methods with AWS CLI

This guide explains how to perform the basic **CRUD operations** on Amazon S3 (Simple Storage Service) using the AWS Command Line Interface (CLI).  
Each step includes commands with explanations so it’s easier to understand what’s happening.

---


# CRUD Methods in Detail

## 1. What is CRUD?

**CRUD** is an acronym for the four most common actions you can perform with data in any storage system:

1. **Create** → Add new data or resources.  
2. **Read** → Retrieve or view existing data.  
3. **Update** → Modify or replace existing data.  
4. **Delete** → Remove data permanently.  

### Why is CRUD important?
- CRUD forms the **foundation of all applications** — whether you’re dealing with a database, a file system, or a cloud service like AWS S3.  
- Every software project involves some kind of CRUD operations. Example:  
  - **Instagram:** Upload a photo (Create), view feed (Read), edit a caption (Update), delete a post (Delete).  
  - **Email:** Write a new email (Create), read inbox (Read), edit a draft (Update), delete an email (Delete).  

### Everyday Analogy
Think of CRUD like managing documents in a filing cabinet:  
- **Create** → Add a new document into a folder.  
- **Read** → Open a document and read it.  
- **Update** → Replace an older version with an updated one.  
- **Delete** → Shred and remove the document.  

---

## 2. CRUD in Amazon S3

Amazon S3 (**Simple Storage Service**) is AWS’s cloud storage system. Instead of traditional folders, it uses:  
- **Buckets** → Like main folders or containers.  
- **Objects** → The files you upload (images, text files, videos, etc.).  

### How CRUD applies in S3

| CRUD Operation | S3 Equivalent | AWS CLI Example | Explanation |
|----------------|---------------|-----------------|-------------|
| **Create** | Make a new bucket or upload a file | `aws s3 mb s3://mybucket` <br> `aws s3 cp file.txt s3://mybucket` | Adds a new bucket or object. |
| **Read** | List or download buckets/files | `aws s3 ls` <br> `aws s3 cp s3://mybucket/file.txt .` | Lets you see or retrieve existing data. |
| **Update** | Replace a file by re-uploading | `aws s3 cp newfile.txt s3://mybucket/file.txt` | Uploading with the same name overwrites the old version (unless versioning is enabled). |
| **Delete** | Remove files or buckets | `aws s3 rm s3://mybucket/file.txt` <br> `aws s3 rb s3://mybucket --force` | Permanently deletes resources. |

---

### 2.1 Create in S3
- **Create a bucket** (like making a new folder in the cloud):  
  ```bash
  aws s3 mb s3://my-first-bucket


* **Upload a file** (place an object inside the bucket):

  ```bash
  aws s3 cp report.pdf s3://my-first-bucket
  ```

*Note:* Bucket names must be globally unique (no two people in the world can have the same bucket name).

---

### 2.2 Read in S3

* **List all buckets** (see all your folders in S3):

  ```bash
  aws s3 ls
  ```
* **List files in a bucket**:

  ```bash
  aws s3 ls s3://my-first-bucket
  ```
* **Download a file**:

  ```bash
  aws s3 cp s3://my-first-bucket/report.pdf .
  ```

 *Note:* "Read" in S3 doesn’t mean editing the file — it just means retrieving or viewing it.

---

### 2.3 Update in S3

Unlike databases where you can update fields directly, in S3:

* You **replace** a file by uploading a new version with the same name.
* If **versioning** is turned on in your bucket, old versions are kept. Otherwise, the old file is overwritten.

Example:

```bash
aws s3 cp new-report.pdf s3://my-first-bucket/report.pdf
```

This command replaces the old `report.pdf` with the new one.

---

### 2.4 Delete in S3

* **Delete one object (file):**

  ```bash
  aws s3 rm s3://my-first-bucket/report.pdf
  ```
* **Delete all objects in a bucket:**

  ```bash
  aws s3 rm s3://my-first-bucket --recursive
  ```
* **Delete the bucket itself:**

  ```bash
  aws s3 rb s3://my-first-bucket --force
  ```

*Warning:* Deleting is permanent. Once a file is deleted from S3, it cannot be recovered unless you enabled **versioning**.

---

## 3. What Are Ad Hoc Commands?

**Ad hoc commands** are quick, one-off commands that you run in the AWS CLI without writing a full script or playbook.

* They are **manual** and used for **testing, troubleshooting, or quick tasks**.
* Unlike automation (e.g., scripts or Terraform), ad hoc commands are typed in one at a time.

### Examples of Ad Hoc S3 Commands

* Create a bucket:

  ```bash
  aws s3 mb s3://test-bucket-123
  ```
* Upload a file:

  ```bash
  aws s3 cp photo.jpg s3://test-bucket-123
  ```
* List buckets:

  ```bash
  aws s3 ls
  ```
* Delete a file:

  ```bash
  aws s3 rm s3://test-bucket-123/photo.jpg
  ```

### Why use Ad Hoc commands?

* **Learning and testing** → When you’re just trying out AWS features.
* **One-time fixes** → Quick operations without needing to automate.
* **Exploration** → Get comfortable with AWS CLI before writing scripts.

---

## 4. Why CRUD + Ad Hoc Matters in S3

* CRUD is the **universal framework** for working with data.
* With S3, CRUD gives you **full control** over files and buckets.
* Ad hoc commands let you experiment quickly without needing advanced automation tools.

### Example Flow in Real Life:

1. **Create** → You make a bucket `my-project-bucket` and upload `app.log`.
2. **Read** → You list the bucket to confirm `app.log` is uploaded.
3. **Update** → You upload a new version of `app.log` to overwrite the old one.
4. **Delete** → You remove the file or delete the bucket once the project ends.

---

##  Summary

* **CRUD = Create, Read, Update, Delete.**
* In **S3**, these apply to **buckets** (storage containers) and **objects** (files).
* **Ad hoc commands** = quick one-off CLI commands (great for practice/testing).
* Mastering CRUD + ad hoc S3 commands is the first step to **understanding automation** (like writing scripts, Terraform, or Ansible playbooks later).

---

## Initial Setup

Before we start using S3, we need to set up the AWS CLI inside an EC2 instance.

1. **Launch a new EC2 instance** from the AWS Console.  
2. **SSH into the instance** from your local machine (e.g., Git Bash on Windows).  
3. **Install pip** (Python’s package installer):  
   ``sudo apt-get install python3-pip -y``

4. **Install AWS CLI** with pip:

   ```bash
   sudo pip install awscli
   ```
5. **Configure AWS CLI** with your account credentials:

   ```bash
   aws configure
   ```

   You’ll be prompted for:

   * **Access Key ID** → From your AWS account.
   * **Secret Access Key** → From your AWS account.
   * **Default region** → Example: `eu-west-1`.
   * **Output format** → Usually `json`.

---

## Creating and Listing a Bucket

Buckets are like folders in the cloud that store your files.

1. **List existing buckets** (to confirm AWS CLI is working):

   ```bash
   aws s3 ls
   ```
2. **Create a new bucket** (name must be globally unique):

   ```bash
   aws s3 mb s3://tech508-rubaet-first-bucket
   ```
3. **Check if your bucket exists** (it should be empty at first):

   ```bash
   aws s3 ls s3://tech508-rubaet-first-bucket
   ```

---

## Uploading Files (Create & Read)

Let’s try adding (creating) files in S3 and reading them back.

1. **Make a local file:**

   ```bash
   echo "this is the first line in a test file" > test.txt
   ```
2. **Check the file contents:**

   ```bash
   cat test.txt
   ```
3. **Upload the file to your bucket:**

   ```bash
   aws s3 cp test.txt s3://tech508-rubaet-first-bucket
   ```
4. **List files in your bucket:**

   ```bash
   aws s3 ls s3://tech508-rubaet-first-bucket
   ```
5. **Try another file:**

   ```bash
   echo "this is the second line in a test file" > test2.txt
   cat test2.txt
   aws s3 cp test2.txt s3://tech508-rubaet-first-bucket
   aws s3 ls s3://tech508-rubaet-first-bucket
   ```

---

## Downloading and Syncing Files

Now let’s retrieve files from S3 (the **Read** operation).

1. **Create a local folder to store downloads:**

   ```bash
   mkdir downloads
   cd downloads/
   ```
2. **Sync bucket contents to your local folder:**

   ```bash
   aws s3 sync s3://tech508-rubaet-first-bucket .
   ```

   > The `.` means "download here in the current directory."
3. **Check the files are downloaded:**

   ```bash
   ls
   ```

---

## Deleting Files and Buckets (Delete)

Be careful with these commands — deletions in S3 are permanent!

1. **Delete a single file:**

   ```bash
   aws s3 rm s3://tech508-rubaet-first-bucket/test.txt
   ```
2. **Check remaining files:**

   ```bash
   aws s3 ls s3://tech508-rubaet-first-bucket
   ```
3. **Re-upload the file (for practice):**

   ```bash
   aws s3 cp test.txt s3://tech508-rubaet-first-bucket
   ```
4. **Delete everything in the bucket (recursive):**

   ```bash
   aws s3 rm s3://tech508-rubaet-first-bucket --recursive
   ```
5. **Check if bucket is empty:**

   ```bash
   aws s3 ls s3://tech508-rubaet-first-bucket
   ```
6. **Re-upload a file before final deletion:**

   ```bash
   aws s3 cp test.txt s3://tech508-rubaet-first-bucket
   ```
7. **Delete the bucket completely:**

   ```bash
   aws s3 rb s3://tech508-rubaet-first-bucket --force
   ```
8. **Verify it’s gone:**

   ```bash
   aws s3 ls
   ```

---

## Extra Notes & Troubleshooting

* **Get help for any AWS CLI command:**

  ```bash
  aws --help
  ```
* **Check Python version:**

  ```bash
  python3 --version
  ```
* **Check AWS CLI version:**

  ```bash
  aws --version
  ```
* **If your system uses `python` as another version, create an alias:**

  ```bash
  alias python=python3
  ```

---

