

10-WEEK GENAI BACKEND PLAN: NESTJS + AWS + GEMINI
FOR: Node/Nest Dev, AWS Console Only, Docker Beginner
STYLE: Deep Concept -> Example -> Code -> Task
GOAL: Build 2 Real Projects: RAG API + Agent System


[PREREQUISITES CHECKLIST]
[ ] Node 20 LTS + NestJS CLI
[ ] Docker Desktop Installed  
[ ] AWS Account + IAM User with Access Key
[ ] Gemini API Key from Google AI Studio
[ ] Git + VSCode + Postman

[COST BUDGET] $70 for 2 months. We will shut down AWS daily.

WEEK 0: DOCKER + AWS SDK CRASH COURSE


--- DAY 1: DOCKER IN 1 DAY ---
CONCEPT: Docker = "Shipping container for code". Code + Node + Deps in 1 box
WHY: So "it works on my machine" becomes "it works on AWS"
EXAMPLE: Instead of installing Postgres on laptop, we run 1 command

CODE:
1. docker run hello-world
2. docker run -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres:16
3. docker-compose up

TASK: 
1. Install Docker Desktop
2. Create docker-compose.yml with: NestJS + Postgres + Redis + Qdrant
3. `docker-compose up` and see all 4 running

--- DAY 2: AWS SDK + S3 + IAM ---
CONCEPT: AWS SDK = Control AWS from Node code instead of Console
WHY: All deployment will be via code
EXAMPLE: `s3.putObject()` instead of clicking "Upload"

CODE: NestJS service to upload file to S3 using @aws-sdk/client-s3

TASK:
1. Create IAM User with S3FullAccess
2. Code: Upload + Download + List files from S3
3. Test with Postman

--- DAY 3: AWS SDK + LAMBDA + SQS ---
CONCEPT: Lambda = Run code without server. SQS = Queue to talk async
WHY: For async doc processing in RAG

CODE: NestJS -> Send message to SQS. Lambda reads it.

TASK:
1. Create SQS Queue in Console
2. Code: Send message from NestJS
3. Code: Lambda function that logs the message

--- DAY 4: AWS RDS + SECRETS MANAGER ---
CONCEPT: RDS = Managed Postgres. Secrets = Store API keys safely
TASK:
1. Create free-tier RDS Postgres
2. Connect NestJS to it
3. Store Gemini API key in Secrets Manager. Read from NestJS

WEEK 1: GENAI CONCEPTS - FROM ZERO


--- DAY 5: WHAT IS AN LLM? TOKENS + PROMPTS ---
CONCEPT: LLM = Pattern matching machine. Token = ~0.75 words. $ = per token
EXAMPLE: "Hello world" = 2 tokens. 1M tokens = $5
CODE: Raw fetch call to Gemini API. No SDK. See JSON
TASK: Build CLI: `npm run ask "what is RAG"` and stream answer

--- DAY 6: WHAT ARE EMBEDDINGS? ---
CONCEPT: Turn text into 1000 numbers. Similar meaning = similar numbers
EXAMPLE: "King" - "Man" + "Woman" = "Queen" in vector space
CODE: Get embedding for "apple" and "orange". Calculate cosine similarity
TASK: Find which is more similar to "fruit": "car" or "banana"

--- DAY 7: WHAT IS A VECTOR DATABASE? ---
CONCEPT: Normal DB = WHERE id=1. Vector DB = "Find docs similar to this"
WHY: We can't do similarity search in Postgres fast
CODE: Install Qdrant with Docker. Add 3 docs. Search with 1 query
TASK: Build mini search: "Find me doc about cats" from 10 docs

--- DAY 8: WHAT IS RAG? THE 4 STEPS ---
CONCEPT: Retrieval Augmented Generation = LLM + Your Docs
ANALOGY: LLM = Student. RAG = Give student the textbook during exam
4 STEPS: Load -> Chunk -> Embed -> Retrieve -> Answer
CODE: 0. Just draw the diagram
TASK: Explain RAG to a 5 year old in 3 lines

--- DAY 9: WHAT IS CHUNKING? ---
CONCEPT: Can't embed 500 page PDF. Split into 500 chunks of 500 tokens
PROBLEM: Bad chunk = "The patient had" and "no cancer" in different chunks
CODE: 3 chunkers: Fixed, Recursive, Semantic
TASK: Chunk 1 PDF and check which keeps context best

WEEK 2: LANGCHAIN.JS - MAKING IT EASY


--- DAY 10: WHAT IS LANGCHAIN.JS? ---
CONCEPT: Pre-built Legos for GenAI. So we don't write 200 lines
EXAMPLE: Our Day 7 code was 50 lines. LangChain does it in 5
CODE: Rebuild Day 7 with @langchain/community
TASK: Convert your manual RAG to LangChain RAG

--- DAY 11: DOCUMENT LOADERS + TEXT SPLITTERS ---
CONCEPT: Loaders read PDF, Web, S3. Splitters do Day 9
CODE: Load PDF from S3 -> Split -> Store in Qdrant
TASK: API: POST /upload that triggers this

--- DAY 12: RETRIEVERS + VECTORSTORES ---
CONCEPT: Retriever = "Smart search". VectorStore = Qdrant/OpenSearch wrapper
CODE: Build retriever that returns top 5 docs with score
TASK: Add filter: "search only user_id=123 docs"

--- DAY 13: MEMORY + CONVERSATIONAL RAG ---
CONCEPT: How chatbot remembers "What did I ask 2 messages ago"
CODE: ConversationBufferMemory with Postgres
TASK: Chat API that remembers last 5 turns

--- DAY 14: HOW TO TEST RAG? EVALS ---
CONCEPT: "Is answer correct?" "Did it use right doc?"
METRICS: Faithfulness, Answer Relevance
CODE: Create 10 Q&A. Auto test your RAG
TASK: If score < 80%, fail the test

WEEK 3-4: PROJECT 1 - RAG API WITH NESTJS ON AWS


WEEK 3: BUILD
DAY 15: NestJS Setup. Modules: GeminiModule, RAGModule
DAY 16: API 1: POST /upload -> S3 -> SQS
DAY 17: Lambda: Read SQS -> Chunk -> Embed -> OpenSearch
DAY 18: API 2: POST /chat -> RAG -> Return with citations
DAY 19: Add Cognito Auth + Rate Limiting
DAY 20: Add Redis Caching for embeddings

WEEK 4: DEPLOY + POLISH
DAY 21: Move Qdrant to OpenSearch Serverless on AWS
DAY 22: Dockerize NestJS. Push to ECR
DAY 23: Deploy to ECS Fargate. Connect to RDS
DAY 24: Add Langfuse. See every LLM call + cost
DAY 25: Load test with k6. Fix. Write README + Architecture Diagram

WEEK 5: AGENTS - CONCEPT FIRST


--- DAY 26: WHAT IS AN AGENT? ---
CONCEPT: Agent = LLM + Tools + Loop. Can "DO" things
EXAMPLE: RAG answers. Agent can "read logs" + "create Jira ticket"
CODE: Make Gemini call function: get_weather("Thane")
TASK: Build 3 tools: calculator, search, send_email

--- DAY 27: WHAT IS LANGGRAPH.JS? ---
CONCEPT: LangChain for loops. StateGraph = Nodes + Edges
EXAMPLE: Node1: Think -> Node2: Call Tool -> Node3: Answer -> Back to Node1
CODE: Build ReAct Agent: Thought -> Action -> Observation loop
TASK: Agent that solves "2+2" by calling calculator tool

--- DAY 28: MEMORY + CHECKPOINTS ---
CONCEPT: Pause agent, save state, resume later
CODE: Save graph state to Postgres
TASK: Agent that can stop mid-way and continue

--- DAY 29: MULTI-AGENT SYSTEMS ---
CONCEPT: 1 Supervisor + 3 Workers. Boss delegates
CODE: Supervisor -> ResearchAgent, CodeAgent, SummaryAgent
TASK: "Research AI news" and delegate to 3 agents

--- DAY 30: DEBUGGING AGENTS ---
CONCEPT: Why agents loop, hallucinate tools
TOOLS: Langfuse tracing, max_steps, guardrails
TASK: Break agent on purpose. Then fix it

WEEK 6-7: PROJECT 2 - AI OPS AGENT ON AWS


WEEK 6: BUILD
DAY 31: Tool 1: query_cloudwatch_logs using AWS SDK
DAY 32: Tool 2: create_jira_ticket using Jira API
DAY 33: Tool 3: query_postgres using TypeORM
DAY 34: Build Supervisor Agent in LangGraph
DAY 35: Test: "Error 500 in prod" -> Agent reads logs -> Creates Jira

WEEK 7: DEPLOY
DAY 36: Deploy agent worker to ECS. Trigger via SQS
DAY 37: Save agent memory to RDS
DAY 38: Full tracing to Langfuse. Dashboard
DAY 39: Add Guardrails: "Agent cannot run DELETE SQL"
DAY 40: Final testing + README

WEEK 8-10: HARDENING + FINAL

DAY 41-42: COST: Use Gemini Flash, Cache, Batch embeddings. Cut 70% cost
DAY 43-44: SECURITY: Prompt injection tests. IAM least privilege
DAY 45-46: SCALE: Test 100 concurrent agents. Add more ECS tasks
DAY 47-48: REFACTOR: Clean code, add comments, improve docs
DAY 49: WRITE: "10 things I learned about RAG and Agents"
DAY 50: FINAL DEMO: Record video of both projects working

[DELIVERABLES]
1. Project 1: RAG API on AWS with evals
2. Project 2: Multi-Agent backend on AWS
3. 2 GitHub repos with READMEs
4. Langfuse dashboard with traces




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

## AWS Lambda Limitations

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


### 🧾 1. **Large PDF Generation — Lambda Timeout**

**Problem:**  
We had a Lambda function generating large PDFs. The process was CPU-intensive and took longer than the Lambda timeout limit (15 minutes max).

**Solution:**  
We moved the workload to **ECS Fargate**, which allowed:
- Longer execution time
- More memory and CPU
- Better control over containerized tasks


### ❄️ 3. **Cold Start in VPC Setup**

**Problem:**  
Lambda functions inside a VPC had **cold start delays** due to ENI (Elastic Network Interface) creation and subnet attachment.

**Solution:**  
We enabled **Provisioned Concurrency**:
- Pre-warmed Lambda instances
- Eliminated cold start latency
- Ensured consistent performance for APIs


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


## 🚀 How You Solved It: Provisioned Concurrency

## Provisioned Concurrency in AWS Lambda

Provisioned Concurrency is like pre-warming your Lambda functions.

### 🔧 What It Does:
- Keeps a specified number of Lambda instances always ready to serve requests.
- No cold starts — because the environment is already set up.
- Works even inside a VPC.

### 🧠 Simple Analogy:
Imagine a coffee shop:
- **Cold start** = barista starts grinding beans only when you order.
- **Provisioned concurrency** = barista already has coffee ready before you walk in.

### ✅ Result:
By enabling provisioned concurrency, you:
- Eliminated cold start delays
- Improved performance for VPC-based Lambda functions
- Ensured consistent response times for your APIs or services

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


### 📊 Example:

Let’s say you provision **5 Lambda instances** with **512 MB memory** for **1 hour**:

- You’ll be charged for keeping those 5 instances warm for **3600 seconds**.
- If they also handle requests, you’ll pay for those executions **separately**.


### ✅ Summary:

Provisioned Concurrency is ideal when:

- You need **consistent low-latency**
- Your Lambda is inside a **VPC**
- You’re okay with **paying a bit more** for performance and reliability

---

**Ques : How would you manage query performance in Lambda?**

🚀 Managing Query Performance in AWS Lambda
Optimizing query performance in AWS Lambda involves tuning both the Lambda function and the external services it interacts with (like databases or APIs). Below are best practices and a real-world example from the Smart Feedback Collector project.
✅ Best Practices
1. Use Connection Pooling Wisely

Problem: Lambda is stateless and may spin up multiple instances, each creating new DB connections.
Solution: Use RDS Proxy for relational databases or DynamoDB, which handles scaling better.
Example: In a Node.js Lambda querying MySQL, use RDS Proxy to manage connections efficiently.

2. Optimize Cold Starts

Keep your function size small.
Avoid heavy initialization outside the handler.
Use Provisioned Concurrency for critical APIs.

3. Use Efficient Queries

Avoid SELECT *; fetch only required fields.
Use indexes properly in DynamoDB or RDS.

4. Cache Results

Use Lambda + API Gateway + CloudFront or ElastiCache (Redis) to cache frequent queries.

5. Paginate Large Results

Don’t return thousands of records in one go.
Use pagination or filtering to manage large datasets.

6. Timeouts and Retries

Set appropriate timeouts for DB/API calls.
Use exponential backoff for retries to avoid overwhelming services.


🔧 Real-World Example: Smart Feedback Collector
If you're querying feedback data from DynamoDB:

Use Query instead of Scan for better performance.
Use Global Secondary Indexes (GSI) for filtering by user or date.
Cache frequent queries using Redis or CloudFront.

---

**Ques : Suppose if you want to design an API with Lambda and API Gateway, what are the best practices you are going to follow. Think about scalability.**


⚙️ Best Practices for Designing APIs with Lambda + API Gateway (Scalability)
Designing scalable APIs using AWS Lambda and API Gateway requires thoughtful architecture and configuration. Below are key best practices to ensure performance, security, and maintainability.
✅ Architecture Best Practices
1. Use REST or HTTP APIs

REST API: Rich features, suitable for complex APIs.
HTTP API: Lightweight, faster, and cost-effective for simple use cases.

2. Separate Concerns

Use multiple Lambda functions for different endpoints (e.g., /create, /update, /delete) to isolate logic and scale independently.

3. Use JSON Schema Validation

Validate request payloads at the API Gateway level to reduce unnecessary Lambda invocations and improve performance.

4. Enable Caching

Use API Gateway caching for GET endpoints to reduce latency and backend load.

5. Secure with IAM + Cognito

Use Amazon Cognito for user authentication.
Use IAM roles for secure Lambda execution.

6. Use Throttling and Rate Limiting

Protect backend services from abuse by configuring throttling and rate limits in API Gateway.

7. Monitor with CloudWatch

Set up CloudWatch metrics, alarms, and logs for each endpoint to monitor performance and detect anomalies.


🔧 Real-World Example: E-Commerce API


/products → GET → Cached at API Gateway.

/order → POST → Validated via JSON schema.

/user/login → POST → Authenticated via Cognito.

---

**Ques: How do you enable cross-account communication in AWS?**

How to Enable Cross-Account Communication in AWS?
Cross-account communication allows resources in Account A to interact with resources in Account B.
✅ Common Methods:


Resource-Based Policies:

S3, Lambda, SNS, etc. support resource policies.
Example: Allow Account B to invoke Lambda in Account A.



IAM Roles with Trust Policies:

Create a role in Account A.
Allow Account B to assume that role.
Use sts:AssumeRole.



VPC Peering / Transit Gateway:

For network-level communication.



EventBridge Cross-Account Events:

Send events from Account A to Account B.



🔧 Example:
You want Lambda in Account B to access S3 in Account A:

Add a bucket policy in Account A's S3:

{
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::ACCOUNT_B_ID:role/LambdaExecutionRole"
  },
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::your-bucket-name/*"
}

---

**Ques: How to communicate between two different AWS accounts? What permissions are required?**

✅ Required Permissions:

**IAM Role Trust Policy (in Account A):**

{
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::ACCOUNT_B_ID:root"
  },
  "Action": "sts:AssumeRole"
}

**IAM Role Permissions (in Account A):**

{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::your-bucket-name/*"
}

**Caller in Account B must have permission to sts:AssumeRole.**

---

**Ques: What is Route53? How to route traffic from abc.com to sapana.com in Route53?**

**✅ What is Route 53?**

Amazon Route 53 is a highly available and scalable DNS web service. It’s used for:

Domain registration

DNS routing

Health checks

Traffic management

✅ Routing Traffic from abc.com to sapana.com:

You can achieve this using:


**HTTP Redirection:**

Route 53 itself doesn’t support HTTP redirects.

Use S3 static website hosting with redirect rules.

Steps:

- Create an S3 bucket named abc.com.

- Enable static website hosting.

- Set redirection to sapana.com.

**Update Route 53 DNS:**

Create an A record for abc.com pointing to the S3 website endpoint.

{
  "RedirectAllRequestsTo": {
    "HostName": "sapana.com",
    "Protocol": "https"
  }
}

``

---

**Ques: Disadvantages of CloudFront?**

While CloudFront is powerful, it has some limitations:
❌ Disadvantages:

**Complex Configuration:** : Setting up behaviors, origins, and cache policies can be tricky

**Cost:** : Can be expensive for high traffic if not optimized.

**Propagation Delay:** : DNS and cache invalidation can take time.

**Limited Real-Time Logs:** : Logs are delayed unless using real-time logging (extra cost).

**No Native Redirects:** : You need Lambda@Edge or S3 for redirects.

**Regional Restrictions:** : Some countries may block CloudFront IPs.

---
