
<!-- TOC -->
* [Dashboard Setup and Load Testing Guide](#dashboard-setup-and-load-testing-guide-)
  * [1. Create a CloudWatch Dashboard](#1-create-a-cloudwatch-dashboard-)
  * [2. Perform Load Testing with Apache Benchmark](#2-perform-load-testing-with-apache-benchmark-)
    * [Step 1: SSH into your instance](#step-1-ssh-into-your-instance-)
    * [Step 2: Update and install required tools](#step-2-update-and-install-required-tools)
    * [Step 3: Run a load test with Apache Benchmark](#step-3-run-a-load-test-with-apache-benchmark)
    * [Step 4: Analyze results](#step-4-analyze-results)
  * [Summary](#summary)
<!-- TOC -->

# Dashboard Setup and Load Testing Guide  

This guide covers two parts:  
1. Creating and configuring an AWS CloudWatch Dashboard  
2. Performing load testing on your application using Apache Benchmark (ab)  

---

## 1. Create a CloudWatch Dashboard  

1. **Go to your EC2 instance**  
   - Open the AWS Management Console and select the EC2 instance you want to monitor.  

2. **Enable detailed monitoring**  
   - In the left panel, click **Monitoring**.  
   - Click **Manage detailed monitoring**.  
   - Enable **Detailed Monitoring** and confirm.  
   - This enables 1-minute granularity for CloudWatch metrics instead of the default 5-minute interval.  

3. **Add instance metrics to a dashboard**  
   - On the Monitoring tab, click the **three dots (...)** below any metric graph.  
   - Select **Add to dashboard**.  
   - Choose **Create new dashboard**.  
   - Name the dashboard:  
     ```
     tech508-rubaet-test-dashboard
     ```  
   - Click **Create**, then **Add to dashboard**.  
   - You should now see your instance metrics (CPU, network, disk, etc.) on the new dashboard.  

---

## 2. Perform Load Testing with Apache Benchmark  

### Step 1: SSH into your instance  
Use Git Bash (or another terminal) to connect:  
```bash
ssh -i your-key.pem ubuntu@<EC2-Public-IP>
````

### Step 2: Update and install required tools

Run the following to update packages and install Apache utilities:

```bash
sudo apt update -y && sudo apt upgrade -y
sudo apt install tree -y
sudo apt install apache2-utils -y
```

* `tree`: lets you view directory structures in a tree format.
* `apache2-utils`: provides the `ab` (Apache Benchmark) tool.

### Step 3: Run a load test with Apache Benchmark

Use `ab` to simulate multiple requests to your application:

```bash
ab -n 1000 -c 100 https://<App-VM-IP-Address>/
```

* `-n 1000` → total number of requests to perform (1000 requests).
* `-c 100` → number of concurrent requests (100 users hitting at the same time).
* `https://<App-VM-IP-Address>/` → replace with your app’s public IP or domain.

Example:

```bash
ab -n 1000 -c 200 https://54.210.123.45/
```

This will simulate 1000 requests with 200 concurrent users, which helps you check how your app performs under heavier load.

### Step 4: Analyze results

The output includes:

* **Requests per second** (throughput)
* **Time per request** (latency)
* **Failed requests** (stability issues)

By comparing results with different `-c` values (e.g., 100 vs 200), you can see how well your application scales under load.

---

## Summary

At the end, you will have:

* A CloudWatch dashboard showing real-time performance metrics
* Load test results from Apache Benchmark to validate system performance






