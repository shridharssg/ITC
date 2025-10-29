**1. AWS Services & Architecture**

AWS services used

How do you set up and access an AWS EC2 instance for application deployment?

AWS storage classes

Limitations of Lambda. Have you faced any?

How would you manage query performance in Lambda?

Suppose if you want to design an API with Lambda and API Gateway, what are the best practices you are going to follow. Think about scalability.

How do you enable cross-account communication in AWS?

How to communicate between two different AWS accounts? What permissions are required?

What is Route53? How to route traffic from abc.com to sapana.com in Route53?
Disadvantages of CloudFront?

---

**Ques : AWS services used**

---

How do you set up and access an AWS EC2 instance for application deployment?

✅ Concept:
Amazon EC2 (Elastic Compute Cloud) provides scalable virtual servers. To deploy an application:


- Launch an EC2 instance:

- Choose an AMI (Amazon Machine Image) like Ubuntu or Amazon Linux.

- Select instance type (e.g., t2.micro for free tier).

- Configure network (VPC, subnet).

- Add storage.

- Add tags (optional).

- Configure security group (open ports like 22 for SSH, 80/443 for web).

- Launch with a key pair for SSH access.



**Access the instance:**

Use SSH:

- Shellssh -i "your-key.pem" ec2-user@<public-ip>Show more lines




**Deploy your app:**

- Install Node.js, Nginx, PM2, etc.

- Clone your repo or upload files.

- Start the app using PM2 or systemd.



**📦 Real-time project example:**

In your Smart Feedback Collector, you might deploy a Node.js backend on EC2 to handle admin dashboard requests or SMS API integration.

🔧 Use case:

- Hosting a backend service that needs full control over OS and runtime.

- Running cron jobs or background workers.

- Integrating third-party APIs that require static IPs.

**💡 Best practices:**

- Use Elastic IP for consistent access.

- Enable CloudWatch Agent for monitoring.

- Use Auto Scaling Groups for high availability.

---

**Ques : AWS storage classes**

## 🗄️ AWS S3 Storage Classes Explained Simply

AWS S3 offers different storage classes to help you save money based on how often you access your data. Think of it like choosing the right place to store your stuff depending on how often you need it.

| **Storage Class**             | **Description**                                                                 | **Best For**                                      |
|------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------|
| **S3 Standard**              | Fast and always available.                                                       | Frequently accessed files like images, videos.   |
| **S3 Intelligent-Tiering**  | Automatically moves data between cost-effective tiers based on usage.           | When you're unsure how often data will be used.  |
| **S3 Standard-IA**          | Cheaper than Standard, but slower to access.                                     | Backups or files accessed occasionally.          |
| **S3 One Zone-IA**          | Like Standard-IA but stored in one zone only.                                    | Non-critical data that can be recreated.         |
| **S3 Glacier**              | Very low cost, but takes minutes to hours to retrieve.                           | Long-term archives like logs or compliance data. |
| **S3 Glacier Deep Archive**| Lowest cost, but takes hours to retrieve.                                        | Rarely accessed data, like 7-year-old records.   |
| **S3 Reduced Redundancy**   | Lower durability and availability (deprecated).                                 | Not recommended — use other classes instead.     |

### 💡 Tip:
- Use **Standard** for active data.
- Use **Glacier** or **Deep Archive** for long-term storage.
- Use **Intelligent-Tiering** when you're not sure how often data will be accessed.

## 🗄️ Recommended AWS S3 Storage Class for Old but Quickly Accessible Data

If you have data that is **not accessed frequently** but still needs to be **retrieved quickly when required**, use:

### 🔹 S3 Standard-IA (Infrequent Access)

- ✅ **Lower cost** than S3 Standard
- ✅ **High durability and availability**
- ✅ **Fast retrieval** (no waiting like Glacier)
- ❗ Slightly higher retrieval cost per GB

### 📦 Best For:
- Old logs
- Archived reports
- Historical data
- Backup files

> 💡 Tip: Avoid Glacier or Deep Archive if you need quick access. They are cheaper but take minutes to hours to retrieve.
---


Limitations of Lambda. Have you faced any?

## AWS Lambda Limitations (Explained Simply)

| **Limitation**              | **Explanation**                                                                 | **Real-World Impact / Developer Experience**                                                                 |
|----------------------------|----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| **Timeout Limit**          | Max execution time is 15 minutes.                                               | Long-running tasks (e.g., video processing, large DB queries) can fail. Use ECS or Step Functions instead.   |
| **Memory Limit**           | Max memory is 10 GB.                                                            | Heavy ML models or large data processing may hit this limit.                                                 |
| **Ephemeral Storage**      | Only 512 MB `/tmp` space by default (can be increased to 10 GB).                | Temporary file operations (e.g., image conversion) are restricted.                                           |
| **Cold Starts**            | First request after idle time is slow (especially in VPC).                      | Users may experience delay. Use provisioned concurrency or keep functions warm.                              |
| **Package Size Limit**     | Deployment package (code + dependencies) must be ≤ 250 MB zipped.              | Large Node.js projects may need optimization or use of Lambda Layers.                                        |
| **No Persistent State**    | Lambda is stateless — no memory of previous runs.                              | Use external storage (e.g., DynamoDB, S3) to maintain state.                                                 |
| **Limited Concurrency**    | Default is 1,000 concurrent executions per account (can be increased).          | High-traffic apps may hit this limit and throttle.                                                           |
| **VPC Networking Latency** | Lambda inside VPC has higher cold start latency.                               | Slower performance when accessing RDS or private resources.                                                  |
| **Limited Language Support**| Only supports Node.js, Python, Java, Go, .NET, Ruby officially.                | Custom runtimes require container images, which adds complexity.                                             |
| **Debugging is Harder**    | No direct debugging like breakpoints.                                           | You rely on logs (CloudWatch), which can slow down troubleshooting.                                          |


## ✅ Have I Faced These Challenges?

Yes, and here’s how we tackled them in real projects:

---

### 🧾 1. **Large PDF Generation — Lambda Timeout**

**Problem:**  
We had a Lambda function generating large PDFs. The process was CPU-intensive and took longer than the Lambda timeout limit (15 minutes max).

**Solution:**  
We moved the workload to **ECS Fargate**, which allowed:
- Longer execution time
- More memory and CPU
- Better control over containerized tasks

---

### ❄️ 3. **Cold Start in VPC Setup**

**Problem:**  
Lambda functions inside a VPC had **cold start delays** due to ENI (Elastic Network Interface) creation and subnet attachment.

**Solution:**  
We enabled **Provisioned Concurrency**:
- Pre-warmed Lambda instances
- Eliminated cold start latency
- Ensured consistent performance for APIs

---

### 🔄 4. **Stateful Workflows — Step Functions + DynamoDB**

**Problem:**  
We needed to manage multi-step workflows with state tracking, retries, and branching logic — which is hard to do in plain Lambda.

**Solution:**  
We integrated:
- **AWS Step Functions** for orchestrating workflows
- **DynamoDB** for storing state, intermediate results, and tracking progress

This allowed us to:
- Build reliable, fault-tolerant workflows
- Handle retries and error paths gracefully
- Maintain state across multiple Lambda invocations
---

### 🧩 Summary

| Challenge                     | Solution                        |
|------------------------------|----------------------------------|
| PDF generation timeout       | ECS Fargate                      |
| Puppeteer package too large  | Lambda Layers                    |
| Cold start in VPC            | Provisioned Concurrency          |
| Stateful workflows           | Step Functions + DynamoDB        |


## ❄️ What Is a Cold Start in AWS Lambda?

When a Lambda function is invoked for the first time (or after being idle), AWS needs to:

- Set up the runtime environment
- Download your code
- Attach network interfaces (especially if it's inside a VPC)

This setup takes time — usually a few hundred milliseconds, but in a VPC, it can take several seconds due to the extra step of creating Elastic Network Interfaces (ENIs).

### 🧊 Cold Start + VPC = Slower Response

If your Lambda is inside a VPC, AWS has to:

- Create and attach ENIs to your Lambda
- Connect it to your private subnets
- Wait for the networking to be ready

This causes cold start delays, especially noticeable in APIs or real-time systems.

---

## 🚀 How You Solved It: Provisioned Concurrency

Provisioned Concurrency is like pre-warming your Lambda


## 💰 Does Provisioned Concurrency Increase Cost?

Yes — enabling **Provisioned Concurrency** increases cost because AWS keeps a certain number of Lambda instances **always warm and ready**, even if they’re not actively handling requests.

### 💸 Cost Breakdown (Simplified)

Provisioned Concurrency adds **two types of charges**:

1. **Provisioned Concurrency Charges**
   - You pay for the number of Lambda instances kept warm.
   - Charged **per second**, based on the **memory size** allocated.

2. **Request & Duration Charges**
   - You still pay for:
     - Number of requests
     - Execution duration (same as regular Lambda)

---

### 📊 Example:

Let’s say you provision **5 Lambda instances** with **512 MB memory** for **1 hour**:

- You’ll be charged for keeping those 5 instances warm for **3600 seconds**.
- If they also handle requests, you’ll pay for those executions **separately**.

---

### ✅ Summary:

Provisioned Concurrency is ideal when:

- You need **consistent low-latency**
- Your Lambda is inside a **VPC**
- You’re okay with **paying a bit more** for performance and reliability

---

How would you manage query performance in Lambda?

---

Suppose if you want to design an API with Lambda and API Gateway, what are the best practices you are going to follow. Think about scalability.

---

How do you enable cross-account communication in AWS?

---

How to communicate between two different AWS accounts? What permissions are required?

---

What is Route53? How to route traffic from abc.com to sapana.com in Route53?

---

Disadvantages of CloudFront?

---
