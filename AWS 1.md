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
