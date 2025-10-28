ques covers

Ques 1. designing the API to ensuring it's functional, secure, and scalable.

Ques 2. If api is taking too long, how to deal

Ques 3. As a developer. How you think in testing point of view.
other covers : Testing Types, BDD,TDD, 

Ques 4. What are the best practices to be followed while coding

Ques 5. How you interact with other team members if they are working on same architecture and there is a dependency.

Ques 6. What are the monitoring tools you have used?

Ques 7. What frontend and backend deployment services did you use in your most recent project?

Ques 8. How you perform the code review or What do you check during code review
Or How do you approach code reviews, and what key aspects do you focus on during the process?		

---

Ques 1. designing the API to ensuring it's functional, secure, and scalable.

	1. Understand the Requirements and Plan

		Define the Purpose: Clearly define the goal of the API. What data or services will it provide? Who are the intended users (other developers, third-party apps, internal services)?

		Design Endpoints: Identify the various endpoints your API will have and what operations they will perform. For example, a GET /users endpoint could retrieve a list of users, while POST /users could create a new user.

		Choose the Data Format: Most APIs communicate using JSON, but XML or other formats could also be used depending on the use case.


	2. Select a Framework and Tools

		The choice of framework largely depends on the programming language you are comfortable with and the specific needs of your API. Here are some popular frameworks:

		Python: Flask, FastAPI, Django

		JavaScript (Node.js): Express.js, NestJS

		3. Implement Authentication and Authorization

	If your API is exposed to third-party applications, you’ll need to secure it. The most common ways are:

	API Keys: Simple and easy but not as secure as OAuth.

	OAuth 2.0: For more secure authentication and authorization.

	JWT (JSON Web Tokens): A common token-based authentication mechanism.


	4. Use RESTful Principles

		Ensure that your API follows REST (Representational State Transfer) principles:

		Stateless: Each request must contain all the information the server needs to process it.

		Resources: Use nouns for endpoints (e.g., /users, /products), not actions (e.g., /get_users).

		HTTP Methods: Use appropriate HTTP methods:

		GET: Retrieve data.

		POST: Create data.

		PUT: Update data.

		DELETE: Remove data.

	5. Return Proper HTTP Status Codes

		Use correct HTTP status codes to indicate the result of the API request:

		200 OK: Successful GET request.

		201 Created: Successful POST request (data created).

		204 No Content: Successful DELETE request (no content returned).

		400 Bad Request: Invalid input from the client.

		401 Unauthorized: Missing or incorrect API key/token.

		404 Not Found: The requested resource does not exist.

		500 Internal Server Error: Something went wrong on the server.

	6. Testing the API

		You can use tools like Postman or cURL to test your API


	7. Document the API

		A well-documented API is easier for developers to integrate with. Use Swagger/OpenAPI or Postman to generate API documentation automatically.

		Swagger/OpenAPI: Provides a standardized way to describe API endpoints and their inputs/outputs.

		Postman: You can export collections that document the endpoints and provide interactive examples.



	8. Handle Errors Gracefully

		Always handle errors in a way that doesn't expose sensitive information. Create a consistent error response format, such as:

		{
		  "error": "Invalid request",
		  "message": "The provided data is incorrect."
		}

	9. Deploy the API

		Once your API is ready, you can deploy it. Popular deployment options include:

		Heroku: Simple and fast deployment.

		AWS: More robust and scalable, using services like AWS Lambda (for serverless), EC2, or API Gateway.

		Google Cloud: Similar to AWS but focused on Google services.

	10. Maintain and Monitor the API

		Rate Limiting: To avoid abuse and ensure fair use.

		Logging: Track all requests, errors, and access patterns.

		Versioning: As your API evolves, version it to ensure backward compatibility (e.g., /api/v1/users, /api/v2/users).



	This is a high-level overview, but the steps above should give you a strong foundation for creating a functional and secure API
---

Ques 2. If api is taking too long, how to deal


✅ 1. Identify the Root Cause

	Slow database queries: Optimize queries, add indexes.
	Heavy computation: Move to background jobs or use worker threads.
	External API latency: Use caching or asynchronous processing.


✅ 2. Implement Timeouts
	Set a maximum time for requests to complete:
	JavaScriptconst express = require('express');const app = express();app.use((req, res, next) => {  res.setTimeout(10000, () => { // 10 seconds    res.status(503).send('Request timed out');  });  next();});Show more lines

✅ 3. Use Asynchronous Processing
	For long-running tasks, respond immediately and process in the background:

	Queue systems: Use Bull or RabbitMQ.
	Pattern: Return 202 Accepted and provide a status endpoint.


✅ 4. Enable Caching

	Cache frequent responses using Redis or in-memory cache.
	Example: node-cache or express-redis-cache.


✅ 5. Optimize API Design

	Break large tasks into smaller chunks.
	Use pagination for large data sets.


✅ 6. Use Load Balancing & Scaling

	Deploy multiple instances behind a load balancer.
	Use PM2 or Docker for horizontal scaling.


✅ 7. Monitor & Alert

	Use New Relic, Datadog, or AWS CloudWatch to track slow endpoints.



In terms of AWS,

	✅ 1. Use AWS Lambda for Async Processing
			If the API call involves heavy computation or external service latency:

			Trigger an AWS Lambda function asynchronously via SNS, SQS, or Step Functions.
			Respond immediately with 202 Accepted and let Lambda handle the task.
			Store results in DynamoDB or S3, and provide a status endpoint.

		Example Flow:
		Client → API Gateway → Lambda → SQS → Worker Lambda → DynamoDB


	✅ 2. Implement Queues (SQS)
		For long-running tasks:

		Push the request to Amazon SQS.
		A Lambda or ECS service processes the queue.
		This decouples the API from the heavy task.


	✅ 3. Use Caching with AWS Services

		Amazon ElastiCache (Redis) for frequently accessed data.
		API Gateway caching for repeated API calls.
		CloudFront for static content.

	✅ 4. Monitoring & Alerts

		Use AWS CloudWatch to track API latency.
		Set alarms for slow endpoints.

---

Ques 3. As a developer. How you think in testing point of view.


	Thinking like a developer from a testing perspective means shifting your mindset from “Does it work?” to “How can it fail?” Here’s how I approach it:

	my focus not like how to write, or which way it will work instead of that im thinking like how it will fail, 


	✅ 1. Understand the Requirements

	Clarify functional and non-functional requirements.
	Ask: What is the expected behavior? What edge cases exist?


	✅ 2. Think in Terms of Test Types
	For a Node.js + AWS API, I’d consider:


	Unit Tests
	Test individual functions (e.g., Lambda handler logic, utility functions).
	Tools: Jest, Mocha.


	Integration Tests
	Test API endpoints with AWS services (S3, DynamoDB, SQS).
	Tools: Supertest + LocalStack for AWS mocks.


	Performance Tests
	Simulate slow APIs and measure response time.
	Tools: Artillery, JMeter.


	Security Tests
	Validate IAM roles, API Gateway auth, input sanitization.


	Resilience Tests
	Test timeouts, retries, circuit breakers.



	✅ 3. Cover Edge Cases

	Large payloads.
	Missing or invalid headers.
	AWS service throttling or failures.
	Network latency.


	✅ 4. Automate

	CI/CD pipeline with GitHub Actions or AWS CodePipeline.
	Run tests on every commit.


	✅ 5. Think About Observability

	Add structured logging (e.g., Winston).
	Use AWS CloudWatch for metrics and alerts.


	✅ 6. Simulate Real Failures

	Kill services (Chaos testing).
	Inject latency in mocks.
	Test retry logic with exponential backoff.
	
	
	
In terms of Node.js REST API, no AWS.

From a developer’s testing mindset, here’s how I’d approach it:

✅ 1. Think in Layers
Testing isn’t just “does the endpoint return 200?” It’s about validating every layer:

Controller logic (routes, request/response)
Business logic (services)
Data layer (DB queries)
Error handling (timeouts, invalid input)


✅ 2. Types of Tests
For a REST API in Node.js:


Unit Tests
	Test small functions in isolation (e.g., validation, utility functions).
	Tool: Jest or Mocha.


Integration Tests
Test endpoints with DB or external services.
Tool: Supertest.


Functional Tests
Validate full API flow (e.g., POST → GET → DELETE).


Performance Tests
Simulate load and latency.
Tool: Artillery.


Security Tests
Check for SQL injection, XSS, auth flaws.



✅ 3. Edge Cases to Consider

Missing or invalid headers.
Empty body or malformed JSON.
Large payloads.
Slow DB or external API.
Concurrent requests.


✅ 4. Automate

Run tests in CI/CD pipeline.
Use coverage reports to ensure critical paths are tested.


✅ 5. Example: Testing a Slow API
If an API might take too long:

Test timeout behavior.
Test retry logic if implemented.
Test graceful error response.	


---

Ques Testing 

✅ Module: User Management
Endpoints:

POST /users → Create user

GET /users/:id → Get user details

PUT /users/:id → Update user

DELETE /users/:id → Delete user


1. Unit Testing

	Goal: Test individual functions in isolation (no DB, no network).
	Example: Validate user input before saving.
	
	// userValidator.js


	function validateUser(data) {
	  if (!data.name || typeof data.name !== 'string') return false;
	  if (!data.email || !data.email.includes('@')) return false;
	  return true;
	}

	module.exports = validateUser;
	
	const validateUser = require('../utils/userValidator');

	describe('validateUser', () => {
	  it('should return true for valid user', () => {
		expect(validateUser({ name: 'John', email: 'john@example.com' })).toBe(true);
	  });

	  it('should return false for missing email', () => {
		expect(validateUser({ name: 'John' })).toBe(false);
	  });
	});

	
	✅ Why important?
	Catches logic errors early without involving DB or API.



3. Integration Testing
	Goal: Test API endpoint + DB interaction together.
	Example: Test POST /users with MongoDB.	

	const request = require('supertest');
	const app = require('../app');
	const mongoose = require('mongoose');

	beforeAll(async () => {
	  await mongoose.connect('mongodb://localhost:27017/testdb');
	});

	afterAll(async () => {
	  await mongoose.connection.close();
	});

	describe('POST /users', () => {
	  it('should create a user and return 201', async () => {
		const res = await request(app)
		  .post('/users')
		  .send({ name: 'Alice', email: 'alice@example.com' });
		expect(res.status).toBe(201);
		expect(res.body.name).toBe('Alice');
	  });
	});


✅ Why important?
Ensures API works with real DB and middleware.

3. Functional Testing
	Goal: Test full workflow as a user would.
	
	Example: Create → Fetch → Update → Delete.

  describe('User workflow', () => {
		  let userId;

		  it('should create a user', async () => {
			const res = await request(app).post('/users').send({ name: 'Bob', email: 'bob@example.com' });
			userId = res.body._id;
			expect(res.status).toBe(201);
		  });

		  it('should fetch the user', async () => {
			const res = await request(app).get(`/users/${userId}`);
			expect(res.body.name).toBe('Bob');
		  });

		  it('should update the user', async () => {
			const res = await request(app).put(`/users/${userId}`).send({ name: 'Bobby' });
			expect(res.body.name).toBe('Bobby');
		  });

		  it('should delete the user', async () => {
			const res = await request(app).delete(`/users/${userId}`);
			expect(res.status).toBe(204);
		  });
	});

✅ Why important?
Simulates real user flow and catches integration gaps.


4. Performance Testing

	Goal: Check API under load and latency conditions.

	Example: Test GET /users with 1000 concurrent requests using Artillery.


	artillery.yml:
	
	config:
	  target: "http://localhost:3000"
	  phases:
		- duration: 60
		  arrivalRate: 50
	scenarios:
	  - flow:
		  - get:
			  url: "/users"


	✅ Why important?
	Ensures API can handle real-world traffic without timeouts.



Functional testing checks business functionality as a whole, ensuring that the system behaves as expected from a user perspective.
It’s not limited to one module—it covers cross-module interactions.

	Example Scenario
		Imagine an E-Commerce API with these modules:

		User: Register, Login
		Product: Browse, Add to Cart
		Billing: Checkout, Payment

	A functional test case would simulate:

		Register a user → POST /users
		Login and get token → POST /auth/login
		Browse products → GET /products
		Add product to cart → POST /cart
		Checkout and pay → POST /billing/checkout
		
		
Difference Between Test Types

Unit Test: Validate calculateCartTotal() function in isolation.
Integration Test: Test POST /cart with DB interaction.
Functional Test: Simulate full user journey across modules.
Performance Test: Stress test POST /billing/checkout with 1000 concurrent requests.

	
**Load testing**

	Load testing is about simulating high traffic to see how your API performs under stress. For a Node.js REST API, here’s how you can do it step by step:

	✅ 1. Why Load Testing?

	Identify bottlenecks (CPU, DB, network).
	Check response time under heavy load.
	Ensure scalability before production.


	✅ 2. Popular Tools

	Artillery (simple, Node.js-friendly)
	JMeter (Java-based, enterprise)
	k6 (modern, scriptable)
	Locust (Python-based)


	✅ 3. Example with Artillery
	Install:	
	
		npm install -g artillery
			
	Create load-test.yml:	
		config:
		  target: "http://localhost:3000"
		  phases:
			- duration: 60        # Test for 60 seconds
			  arrivalRate: 50     # 50 new users per second
		scenarios:
		  - flow:
			  - get:
				  url: "/users"	
				  
				  
	Run : 
		artillery run load-test.yml
		
	
	Output:

		Requests per second
		Latency (p95, p99)
		Error rate
		
==============================================

Integrating load testing and other test types (unit, integration, functional) into a CI/CD pipeline ensures every code change is validated for correctness and performance before deployment. Here’s a complete approach:

✅ 1. CI/CD Pipeline Overview
	Typical stages:

	Build → Install dependencies, compile code.
	Unit Tests → Fast, isolated checks.
	Integration Tests → Validate API + DB.
	Functional Tests → End-to-end workflows.
	Load/Performance Tests → Stress test critical endpoints.
	Deploy → If all tests pass.


✅ 2. Tools

	CI/CD: GitHub Actions, GitLab CI, Jenkins, AWS CodePipeline.
	Unit/Integration/Functional: Jest + Supertest.
	Load Testing: Artillery or k6.


✅ 3. Example GitHub Actions Workflow
	Create .github/workflows/ci.yml:


	name: Node.js API CI/CD

	on:
	  push:
		branches: [ main ]
	  pull_request:
		branches: [ main ]

	jobs:
	  build:
		runs-on: ubuntu-latest
		steps:
		  - uses: actions/checkout@v3
		  - name: Setup Node.js
			uses: actions/setup-node@v3
			with:
			  node-version: '18'
		  - run: npm install

	  unit-tests:
		runs-on: ubuntu-latest
		needs: build
		steps:
		  - uses: actions/checkout@v3
		  - run: npm test -- --coverage

	  integration-tests:
		runs-on: ubuntu-latest
		needs: unit-tests
		services:
		  mongo:
			image: mongo:5
			ports: ['27017:27017']
		steps:
		  - uses: actions/checkout@v3
		  - run: npm run test:integration

	  functional-tests:
		runs-on: ubuntu-latest
		needs: integration-tests
		steps:
		  - uses: actions/checkout@v3
		  - run: npm run test:functional

	  load-tests:
		runs-on: ubuntu-latest
		needs: functional-tests
		steps:
		  - uses: actions/checkout@v3
		  - run: npm install -g artillery
		  - run: artillery run tests/load-test.yml
		  

✅ 4. How Each Test Fits

	Unit Tests: Run first because they’re fast and catch logic errors early.
	Integration Tests: Spin up DB containers (MongoDB, PostgreSQL) using CI services.
	Functional Tests: Use real API endpoints after integration passes.
	Load Tests: Run last because they take time and resources.


✅ 5. Best Practices

	Thresholds for load tests: Fail pipeline if p95 latency > 500ms or error rate > 1%.
	Parallelization: Run unit and integration tests in parallel for speed.
	Artifacts: Store coverage reports and load test results for analysis.
	Environment: Use staging or ephemeral environments for functional/load tests.		  
	
======================

✅ When Tests Run


During Local Development

	You run unit tests and sometimes integration tests locally before committing.
	This is fast and helps catch errors early.
	Example
	
		npm test        # Runs unit tests
		npm run test:integration
		npm run test:functional
		
	During CI/CD (on push or pull request)

All tests (unit, integration, functional, load) run automatically in the pipeline.
This ensures every commit is validated before merging or deploying.
Example: GitHub Actions workflow triggers on push or PR.



Before Deployment

Pipeline runs full suite, including load tests and security checks.
If any test fails, deployment is blocked.


✅ Recommended Flow

Local: Unit + quick integration tests.
CI/CD: Full suite (unit → integration → functional → load).
Deployment: Only happens if all tests pass.


✅ Why Not Run Load Tests Locally?

Load tests require high concurrency and can stress your machine.
Better to run them in CI/CD or staging environment.


✅ How to Configure in CI/CD

Use GitHub Actions or Jenkins to:

Trigger tests on push/PR.
Run unit tests first (fast).
Run integration + functional tests in parallel.
Run load tests last (optional for every commit, mandatory before release).	


✅ Pipeline Design
1. Trigger Conditions

Push to any branch → Run unit tests.
Pull Request to main → Run unit + integration + functional tests.
Deployment (tag or release) → Run load tests.

2. Example GitHub Actions Workflow
Create .github/workflows/ci.yml:

name: Node.js API CI/CD

on:
  push:
    branches: ['*']   # Every commit
  pull_request:
    branches: [ main ] # PR to main
  workflow_dispatch:   # Manual trigger for deployment
  release:
    types: [published] # On release

jobs:
  # ✅ Unit Tests on every commit
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm install
      - run: npm test -- --coverage

  # ✅ Integration + Functional Tests on PR
  integration-functional-tests:
    runs-on: ubuntu-latest
    needs: unit-tests
    if: github.event_name == 'pull_request'
    services:
      mongo:
        image: mongo:5
        ports: ['27017:27017']
    steps:
      - uses: actions/checkout@v3
      - run: npm install
      - run: npm run test:integration
      - run: npm run test:functional

  # ✅ Load Tests only before deployment
  load-tests:
    runs-on: ubuntu-latest
    needs: integration-functional-tests
    if: github.event_name == 'release'
    steps:
      - uses: actions/checkout@v3
      - run: npm install -g artillery
      - run: artillery run tests/load-test.yml
``


✅ 3. npm Scripts Setup
In package.json:


"scripts": {
  "test": "jest --coverage",
  "test:integration": "jest --config jest.integration.config.js",
  "test:functional": "jest --config jest.functional.config.js"
}

✅ 4. Best Practices

Thresholds for load tests:

Fail if p95 latency > 500ms or error rate > 1%.
Artillery supports expect rules



ensure:
  - p95: 500
  - errorRate: 1
  
==========

When test cases is updated


✅ Why Do Tests Need Updates?

Code changes = behavior changes
If you add a new feature, modify logic, or change API response format, your existing tests may fail or become irrelevant.
Prevent false positives/negatives
Outdated tests can give misleading results (e.g., passing when they shouldn’t).
Maintain coverage
New code paths need new tests to keep coverage high.


✅ When to Update Tests

Add new endpoints or modules → Write new unit/integration/functional tests.
Change request/response schema → Update functional tests.
Refactor logic → Update unit tests.
Performance improvements → Adjust load test thresholds.



**TDD and BDD are development approaches, **

TDD (Test-Driven Development) and BDD (Behavior-Driven Development) are how you write code and tests during development:

TDD: Write unit tests first, then code.
BDD: Write behavior scenarios first, then code.


Unit, Integration, Functional tests are types of tests that you run in CI/CD pipelines.



✅ What is TDD (Test-Driven Development)?
Think of TDD like writing the exam questions before writing the answers.
Steps:

Write a test first for what the code should do.
Run the test → It fails (because code doesn’t exist yet).
Write the code to make the test pass.
Refactor if needed.

Example:
You need a function to calculate the total price of items in the cart.

Step 1: Write test first

	// cart.test.js
	describe('calculateCartTotal', () => {
	  it('should return 100 for items [50, 50]', () => {
		expect(calculateCartTotal([50, 50])).toBe(100);
	  });
	});

Step 2: Write code  

	function calculateCartTotal(items) {
	  return items.reduce((sum, price) => sum + price, 0);
	}
	
✅ Why TDD?
You build small pieces of logic correctly from the start.

✅ What is BDD (Behavior-Driven Development)?
Think of BDD like writing a story of how the user interacts with the system.
Focus: Business behavior, not just code.


Example Scenario:
Feature: Checkout
  Scenario: Successful checkout
    Given a user has items in the cart
    When they click "Pay"
    Then the payment should succeed and show "Order Confirmed"


BDD Test (using Jest or Cucumber):

	describe('Checkout Flow', () => {
	  it('should confirm order after payment', async () => {
		const res = await request(app)
		  .post('/checkout')
		  .send({ userId: '123', cart: [{ id: 'p1', price: 50 }] });
		expect(res.status).toBe(200);
		expect(res.body.message).toBe('Order Confirmed');
	  });
	});


✅ Why BDD?
It ensures the app behaves as the business expects, and even non-developers can understand the test.


✅ How They Fit Together

Use TDD for internal logic (e.g., price calculation, discount rules).
Use BDD for workflows (e.g., user adds product → checkout → payment success).

---

Ques : What are the best practices to be followed while coding

or What strategies do you follow to maintain and enhance code quality?



1. Follow a Consistent Coding Standard
✅ 1. Code Quality

Follow a style guide (e.g., Airbnb for JavaScript).
Use linters like ESLint to enforce consistency.
Keep functions small and focused (Single Responsibility Principle).


✅ 2. Write Clean, Readable Code

Use meaningful names for variables, functions, and classes.
Avoid magic numbers; use constants.
Add comments only where necessary (code should be self-explanatory).


✅ 3. Error Handling

Always handle errors gracefully:

try {
  const user = await User.findById(id);
  if (!user) throw new Error('User not found');
} catch (err) {
  res.status(500).json({ error: err.message });
}


Use centralized error middleware in Express.


✅ 4. Security

Validate all inputs (e.g., express-validator).
Sanitize data to prevent XSS/SQL injection.
Use HTTPS and secure headers (helmet).


✅ 5. Performance

Use pagination for large datasets.
Cache frequently accessed data (Redis).
Optimize DB queries (indexes, projections).


✅ 6. Testing / Implement Automated Testing

Write unit tests for logic.
Integration tests for API + DB.
Functional tests for workflows.
Automate tests in CI/CD.


✅ 7. Documentation

Use Swagger/OpenAPI for API docs.
Keep README updated with setup instructions.


✅ 8. Version Control

Commit often with meaningful messages.
Use feature branches and pull requests.


✅ 9. Environment Management

Use .env for secrets (never hardcode).
Separate dev, test, and prod configs.


✅ 10. Deployment

Use CI/CD pipelines for automated testing and deployment.
Monitor with tools like PM2, CloudWatch, or New Relic.

✅ 10. Use Code Reviews

		Peer reviews before merging code.
		Check for:

		Logic correctness
		Security vulnerabilities
		Performance issues
		Readability


✅ 5. Apply Static Analysis

Use SonarQube or CodeQL for detecting bugs and vulnerabilities.
Integrate these tools into CI/CD.


✅ 6. Keep Dependencies Updated

Regularly update npm packages.

Use npm audit to fix security issues.

---

**Ques : How you interact with other team members if they are working on same architecture and there is a dependency.**


When multiple team members work on the same architecture with dependencies, collaboration and communication become critical. Here’s how you should interact effectively:

	✅ 1. Clear Communication

	Daily Stand-ups: Share what you’re working on and any blockers.
	Dependency Alerts: If your code depends on another module, inform the owner early.
	Use tools like Slack, Microsoft Teams, or Jira comments for quick updates.


	✅ 2. Define Interfaces Early

	Agree on API contracts (request/response format) before coding.
	Use Swagger/OpenAPI or Postman collections to document endpoints.
	Example:

	User Service provides /users/:id
	Product Service consumes that endpoint → both teams agree on schema.


	✅ 3. Use Feature Branches

	Each team works on separate branches.
	Merge only after code review + tests pass.
	Avoid direct commits to main.


	✅ 4. Mock Dependencies

	If another team’s service isn’t ready:

	Use mock servers (e.g., json-server, Postman mock).
	Example: If Billing API isn’t ready, mock its response for integration tests.


	✅ 5. Shared Testing Strategy

	Agree on unit, integration, functional test coverage.
	Example:

	User team writes unit tests for user logic.
	Product team writes integration tests for product + user API calls.
	Functional tests cover full flow (User → Product → Billing).


	✅ 6. Versioning & Contracts

	Use semantic versioning for shared libraries.
	Maintain API versioning (/v1/users).


	✅ 7. CI/CD Integration

	Each team’s pipeline runs unit tests.
	Shared staging environment for integration + functional tests.
	Example:

	User team deploys to staging → Product team runs tests against it.


	✅ 8. Conflict Resolution

	If dependency changes break your code:

	Raise an issue immediately.
	Suggest backward compatibility or feature flags.
	
  ---


**ques : What are the monitoring tools you have used?**


		✅ 1. PM2

		What it does: Process manager for Node.js with built-in monitoring.
		Features:

		CPU and memory usage tracking.
		Auto-restart on crashes.
		Logs aggregation.


		Use case: Monitor API health and resource usage in production.


		✅ 2. New Relic

		What it does: Application performance monitoring (APM).
		Features:

		Tracks response times, throughput, and error rates.
		Detailed transaction traces.
		Alerts for slow endpoints.


		Use case: Detect bottlenecks in API performance.


		✅ 4. AWS CloudWatch (if hosted on AWS)

		What it does: Collects logs and metrics from AWS resources.
		Features:

		Custom dashboards.
		Alarms for latency or errors.

		Use case: Monitor EC2, Lambda, and API Gateway performance.

		✅ 3. Datadog

		What it does: Full-stack monitoring and observability.
		Features:

		Metrics, logs, and traces in one dashboard.
		Real-time alerts.
		Integration with CI/CD pipelines.


		Use case: Monitor distributed systems and microservices.


		✅ 5. Grafana + Prometheus

		What it does: Open-source monitoring stack.
		Features:

		Prometheus collects metrics.
		Grafana visualizes them in dashboards.


		Use case: Monitor API latency, DB queries, and system health.

---    
**Ques : What frontend and backend deployment services did you use in your most recent project?**


✅ Backend Deployment Services

	AWS ECS (Fargate)

		Containerized Node.js apps.
		No need to manage servers.

	Docker + EC2

		Full control for custom deployments.


✅ CI/CD Integration

	GitHub Actions or GitLab CI for:

		Running tests (unit, integration, functional).
		Building and deploying to Vercel/Netlify for frontend.
		Deploying backend to AWS
		
---

**Ques : How you perform the code review / What do you check during code review
Or How do you approach code reviews, and what key aspects do you focus on during the process?**


“How you perform code review” → This is about your process (steps you take: read PR, check design, dive into code, give feedback).
“What do you check during code review” → This is about specific aspects you look for in the code.


✅ What I Check During Code Review (Key Points)

Correctness – Does the code meet requirements? Are edge cases handled?
Readability – Clear naming, proper formatting, minimal comments.
Maintainability – DRY principle, modular structure, no hardcoding.
Performance – Efficient loops, optimized DB queries, pagination.
Security – Input validation, no sensitive data in logs, proper auth.
Error Handling – Centralized error middleware, no silent failures.
Testing – Unit, integration, functional tests updated and passing.
Compliance – Follows coding standards (e.g., Airbnb JS guide).
Impact – Does it break other modules? Backward compatibility?
Deployment readiness – Config and environment variables handled.



✅ How I Perform Code Review

Understand context (feature/bug).

Review high-level design first.

Check code line by line for above points.

Give constructive feedback with examples.

Approve or request changes.


	A code review is not just about checking syntax—it’s about ensuring quality, maintainability, and correctness

	✅ 1. Understand the Context

	Read the PR description or Jira ticket.
	Understand why the change was made (feature, bug fix, refactor).
	Check related files or modules for impact.


	✅ 2. Review High-Level Design

	Does the solution fit the architecture?
	Is the approach scalable and maintainable?
	Are dependencies handled properly?


	✅ 3. Dive into the Code
	I check for:
	a) Correctness

	Does the code meet the requirements?
	Are edge cases handled?
	Are tests updated and passing?

	b) Readability

	Clear variable and function names.
	Consistent formatting (ESLint/Prettier).
	Minimal comments (only where logic is complex).

	c) Maintainability

	No duplicate code (DRY principle).
	Functions/classes follow Single Responsibility Principle.
	Proper modularization.

	d) Performance

	Avoid unnecessary loops or DB calls.
	Use pagination for large data sets.
	Optimize queries and API responses.

	e) Security

	Input validation and sanitization.
	No sensitive data in logs.
	Proper authentication/authorization checks.

	f) Error Handling

	Graceful error responses.
	Centralized error middleware.
	No silent failures.

	g) Testing

	Unit tests for new logic.
	Integration tests for API changes.
	Functional tests for workflows.


	✅ 4. Provide Constructive Feedback

	Be specific: “Consider using async/await for better readability.”
  
	Suggest alternatives if needed.
	
  Avoid personal tone; focus on code quality.


	✅ 5. Approve or Request Changes

	Approve if all checks pass.
  
	Request changes with clear reasons and examples.
