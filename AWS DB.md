✅ 2. Database & Data Management

Ques : If you have to backup DB for auditing, how would you do and what services would you use for it?

Ques : Suppose in AWS you can only archive or backup the database for past 1 day. What other approaches can you follow to backup the DB? How your approach would be different if you want to archive it?

Ques : If attacker deletes your database, what are the steps you’ll take to fix it?

Ques : How to use MongoDB for analytics purpose?

Ques : In this scenario, data needs to be retained for a period of 2 years, after which it should be automatically deleted. Additionally, high-speed access to the data is required for the first 6 months.

--- 

**Ques : If you have to backup DB for auditing, how would you do and what services would you use for it?**

Backup Strategy for Auditing in AWS
To ensure database backups for auditing purposes, especially in a Lambda-based architecture, follow these best practices:
✅ Recommended Services:

**Amazon RDS Snapshots** 
Use automated and manual snapshots for relational databases like MySQL, PostgreSQL, etc.


**Amazon DynamoDB Point-in-Time Recovery (PITR)** 
Enables continuous backups for NoSQL databases with recovery to any second in the last 35 days.


**AWS Backup** 
Centralized backup service for RDS, DynamoDB, EFS, EC2, and more. Supports backup vaults and lifecycle policies.


**Amazon S3** 
Store exported backups (e.g., from MongoDB or custom databases) with lifecycle and versioning support.


**Amazon S3 Glacier / Deep Archive** 
Long-term archival storage for audit compliance.


**AWS Lambda + EventBridge (Scheduler)** 
Automate backup jobs and export tasks.


**MongoDB Atlas Backup**
Built-in backup and restore features for managed MongoDB clusters.


Enable S3 Versioning to retain multiple versions of backups.

Set up S3 Lifecycle Policies to transition backups to Glacier after 6 months and delete after 2 years.

**Automation**

Use AWS Lambda and EventBridge to automate daily backups:

- Create a Lambda function that runs mongodump and uploads to S3.

- Schedule it using EventBridge (e.g., every 24 hours).
- Monitor execution using CloudWatch Logs and set up alerts.


🔒 Security & Compliance

- Encrypt backups using S3 Server-Side Encryption (SSE) or AWS KMS.

- Store backups in AWS Backup Vaults with access control.

- Enable MFA Delete on S3 buckets to prevent accidental or malicious deletion.

- Use AWS CloudTrail to audit backup operations.

---

**Ques : Suppose in AWS you can only archive or backup the database for past 1 day. What other approaches can you follow to backup the DB? How your approach would be different if you want to archive it?**

If AWS only allows backup for the past 1 day, consider these alternatives:

Scenario:
AWS only allows backup or archive of the database for the past 1 day. To ensure long-term data retention and audit compliance, alternative strategies must be implemented.

✅ Alternative Backup Approaches


**Scheduled Manual Backups**

Use AWS Lambda with EventBridge (CloudWatch Events) to trigger daily or hourly backup jobs.

For example, use mongodump or mysqldump to export data and upload to Amazon S3.


**Versioned Backups in S3**

Enable S3 Versioning to retain multiple versions of backup files.

Prevent accidental overwrites or deletions.


**Cross-Region Backup**

Replicate backups to another AWS region using S3 Cross-Region Replication.

Adds redundancy and disaster recovery capability.



**Multi-Account Backup Strategy**

Store backups in a separate AWS account with restricted access.

Use resource-based policies and IAM roles for secure cross-account access.


**Use AWS Backup Vaults**

Store backups in Backup Vaults with lifecycle and access policies.

Supports RDS, DynamoDB, EFS, EC2, and more.




🧊 Archival Strategy (Long-Term Storage)

Archiving is different from backup. It focuses on low-cost, long-term storage with infrequent access.

✅ Recommended Services:

- Amazon S3 Glacier / Glacier Deep Archive
- S3 Lifecycle Policies
- AWS Backup with Vault Retention Rules

🛠️ Example: S3 Lifecycle Policy for Archiving

---

**Ques : If attacker deletes your database, what are the steps you’ll take to fix it?**


If an attacker deletes your database, follow these structured steps to recover the data, secure the environment, and prevent future incidents.

**Step 1: Immediate Response**


**Isolate the Environment**
Disable access to the affected database and revoke suspicious IAM credentials. Stop all services interacting with the compromised database.


**Enable Multi-Factor Authentication (MFA)**
Enforce MFA for all IAM users and root accounts to prevent unauthorized access.



**Step 2: Restore the Database**

- Restore from Backup

- Use the latest RDS Snapshot, DynamoDB Point-in-Time Recovery (PITR), or S3 backup to restore the database.

For MongoDB, use the latest mongodump or Atlas backup.

Shell# Example MongoDB restoremongorestore --uri="mongodb://username:password@host:port/dbname" /backup/dbname-YYYY-MM-DDShow more lines

**Verify Data Integrity** 
Validate restored data against audit logs or checksum files.


**Step 3: Investigate the Attack**


**Enable and Review CloudTrail Logs**
Identify the IAM user or role that performed the delete operation. Check for unusual API calls or access patterns.


**Enable GuardDuty**
Detect ongoing threats or compromised credentials using AWS GuardDuty.



**Step 4: Security Hardening**


- Rotate IAM Credentials and Access Keys

- Immediately rotate all credentials and access keys.

**Restrict IAM Policies**
Apply least privilege principle. Use service control policies (SCPs) in AWS Organizations.


**Enable S3 MFA Delete** 
Prevent unauthorized deletion of backups stored in S3.


**Use Backup Vaults with Access Control**
 Store backups in AWS Backup Vaults with restricted access policies.


**Step 5: Implement Preventive Measures**

**Automate Daily Backups**
Use AWS Lambda + EventBridge to schedule backups. Store backups in versioned S3 buckets with lifecycle policies.


**Cross-Region and Cross-Account Backup Replication**
Add redundancy and protection against region-wide failures or account compromise.


**Set Up Alerts and Monitoring**

Use CloudWatch Alarms for delete operations. Monitor backup success/failure logs and trigger alerts.

---

**Ques : How to use MongoDB for analytics purpose?**

**📊 Using MongoDB for Analytics**
MongoDB is a flexible NoSQL database that supports powerful analytics capabilities through its native Aggregation Framework, integration with BI tools, and support for data visualization platforms.

**✅ Use Cases for Analytics with MongoDB**

Customer behavior analysis
Product performance tracking
Feedback sentiment analysis
Time-series data insights
Real-time dashboards


🔧 Recommended Approaches
**1. MongoDB Aggregation Framework**

Use MongoDB’s built-in aggregation pipeline to perform complex queries and transformations directly on the database.
Example: Average rating per category
JavaScriptdb.feedback.aggregate([  { $match: { createdAt: { $gte: ISODate("2025-01-01") } } },  { $group: { _id: "$category", avgRating: { $avg: "$rating" } } },  { $sort: { avgRating: -1 } }])``Show more lines

**2. MongoDB Charts**

MongoDB Charts is a native visualization tool for creating dashboards directly from MongoDB collections.

No need to export data
Supports filters, aggregation, and sharing
Ideal for internal analytics dashboards

**3. Export to Data Warehouse**
For advanced analytics, export MongoDB data to a data warehouse like:

Amazon Redshift
Google BigQuery
Snowflake

Use ETL tools like Apache NiFi, Airbyte, or MongoDB Connector for BI.

**4. Integration with BI Tools**
Use MongoDB BI Connector to connect with tools like:

Tableau
Power BI
Looker
Excel

This allows SQL-like queries on MongoDB collections.

🗃️ Data Retention Strategy for Analytics
Scenario: Data must be retained for 2 years, with high-speed access for the first 6 months.
Implementation:

Use TTL Index for Auto-Deletion

JavaScriptdb.analytics.createIndex(  { createdAt: 1 },  { expireAfterSeconds: 63072000 } // 2 years)Show more lines

Tiered Storage Strategy


First 6 months: Keep data in primary MongoDB cluster for fast access.
After 6 months: Move older data to cold storage (e.g., Amazon S3 or Glacier).


Scheduled Archival


Use AWS Lambda or cron jobs to export and archive data monthly.

---

**Ques : In this scenario, data needs to be retained for a period of 2 years, after which it should be automatically deleted. Additionally, high-speed access to the data is required for the first 6 months.**

MongoDB Data Retention and Access Strategy
📌 Scenario

Data must be retained for 2 years.
High-speed access is required for the first 6 months.
After 2 years, data should be automatically deleted.


✅ Strategy Overview


Primary Storage (0–6 Months)

Store data in the primary MongoDB cluster.
Ensure indexes are optimized for fast queries.
Use wiredTiger storage engine for high performance.



Secondary Storage (6–24 Months)

Move older data to cold storage (e.g., Amazon S3 or a lower-tier MongoDB cluster).
Use scheduled jobs to export and archive data monthly.



Automatic Deletion After 2 Years

Use TTL (Time-To-Live) Index in MongoDB to automatically delete documents after 2 years.

MongoDB will automatically delete documents 2 years after their createdAt timestamp.
TTL indexes are efficient and require no manual cleanup.


🔄 Archival Workflow (After 6 Months)

Use cron jobs or AWS Lambda to:

Query documents older than 6 months.
Export them using mongoexport.
Upload to Amazon S3 with lifecycle policies.

**S3 Lifecycle Policy Example**

{
  "Rules": [
    {
      "ID": "ArchiveMongoData",
      "Prefix": "analytics-archive/",
      "Status": "Enabled",
      "Transitions": [
        {
          "Days": 180,
          "StorageClass": "GLACIER"
        }
      ],
      "Expiration": {
        "Days": 730
      }
    }
  ]
}

- Moves archived data to Glacier after 6 months.

- Deletes it automatically after 2 years.

---
