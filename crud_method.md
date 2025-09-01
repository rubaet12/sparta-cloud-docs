<!-- TOC -->
* [S3 CRUD Methods with AWS CLI](#s3-crud-methods-with-aws-cli)
  * [**Initial Setup**](#initial-setup)
  * [**Creating and Listing a Bucket**](#creating-and-listing-a-bucket)
  * [**Uploading Files (Create & Read)**](#uploading-files-create--read)
  * [**Downloading and Syncing Files**](#downloading-and-syncing-files)
  * [**Deleting Files and Buckets (Delete)**](#deleting-files-and-buckets-delete)
  * [**Extra Notes & Troubleshooting**](#extra-notes--troubleshooting)
<!-- TOC -->

# S3 CRUD Methods with AWS CLI

This guide outlines the essential CRUD (Create, Read, Update, Delete) operations for Amazon S3 using the AWS Command Line Interface.

---

## **Initial Setup**

1.  Create a new EC2 instance.
2.  SSH into the instance using Git Bash.
3.  Install `pip` (the Python package installer) by running:
    ```bash
    sudo apt-get install python3-pip -y
    ```
4.  Install the AWS CLI tool using `pip`:
    ```bash
    sudo pip install awscli
    ```
5.  Configure the AWS CLI with your credentials:
    ```bash
    aws configure
    ```
    * **Input your Access Key:** `[Your AWS Access Key]`
    * **Input your Secret Access Key:** `[Your AWS Secret Access Key]`
    * **Default region name:** `eu-west-1`
    * **Default output format:** `json`

## **Creating and Listing a Bucket**

1.  List all existing S3 buckets to confirm setup:
    ```bash
    aws s3 ls
    ```
2.  Create a new S3 bucket. The name must be globally unique.
    ```bash
    aws s3 mb s3://tech508-rubaet-first-bucket
    ```
3.  List the contents of your newly created bucket. It should be empty.
    ```bash
    aws s3 ls s3://tech508-rubaet-first-bucket
    ```

## **Uploading Files (Create & Read)**

1.  Create a local text file:
    ```bash
    echo "this is the first line in a test file" > test.txt
    ```
2.  View the contents of the file:
    ```bash
    cat test.txt
    ```
3.  Upload the file to your S3 bucket:
    ```bash
    aws s3 cp test.txt s3://tech508-rubaet-first-bucket
    ```
4.  Verify that the file has been uploaded:
    ```bash
    aws s3 ls s3://tech508-rubaet-first-bucket
    ```
5.  Create and upload a second file using the same process:
    ```bash
    echo "this is the second line in a test file" > test2.txt
    cat test2.txt
    aws s3 cp test2.txt s3://tech508-rubaet-first-bucket
    aws s3 ls s3://tech508-rubaet-first-bucket
    ```

## **Downloading and Syncing Files**

1.  Create a new directory to store downloads:
    ```bash
    mkdir downloads
    cd downloads/
    ls
    ```
2.  Download all files from your S3 bucket into the current directory:
    ```bash
    aws s3 sync s3://tech508-rubaet-first-bucket .
    ```
3.  Check the `downloads` folder to confirm the files are there:
    ```bash
    ls
    ```

## **Deleting Files and Buckets (Delete)**

1.  **DANGEROUS COMMAND:** Remove a single file from the bucket.
    ```bash
    aws s3 rm s3://tech508-rubaet-first-bucket/test.txt
    ```
2.  Check if the file has been deleted:
    ```bash
    aws s3 ls s3://tech508-rubaet-first-bucket
    ```
3.  Re-upload the deleted file for the next step:
    ```bash
    aws s3 cp test.txt s3://tech508-rubaet-first-bucket
    aws s3 ls s3://tech508-rubaet-first-bucket
    ```
4.  **VERY DANGEROUS COMMAND:** Remove all files inside the bucket recursively.
    ```bash
    aws s3 rm s3://tech508-rubaet-first-bucket --recursive
    ```
5.  Check to see if the bucket is now empty:
    ```bash
    aws s3 ls s3://tech508-rubaet-first-bucket
    ```
6.  Re-upload a file again to prepare for the final step:
    ```bash
    aws s3 cp test.txt s3://tech508-rubaet-first-bucket
    ```
7.  **FINAL DANGEROUS COMMAND:** Delete the entire bucket. The `--force` flag is required if the bucket is not empty.
    ```bash
    aws s3 rb s3://tech508-rubaet-first-bucket --force
    ```
8.  Verify that the bucket no longer exists:
    ```bash
    aws s3 ls
    ```
    You will see a list of buckets, but yours should be gone.

---

## **Extra Notes & Troubleshooting**

* **Get help for AWS CLI commands:**
    ```bash
    aws --help
    ```
* **Check Python 3 version:**
    ```bash
    python3 --version
    ```
* **Check AWS CLI version:**
    ```bash
    aws --version
    ```
* **Create an alias for Python 3:** If your system uses `python` as a different version, you can create a temporary alias:
    ```bash
    alias python=python3
    ```