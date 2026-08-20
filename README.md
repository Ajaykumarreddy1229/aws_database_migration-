# aws_database_migration-
Migrated PostgreSQL databases from on‑premises to AWS RDS using AWS Database Migration Service (DMS). Implemented secure SSL endpoints, replication instances, and automated data transfer between source and target RDS instances to ensure seamless, encrypted, and efficient migration.
**Project Description**
This project demonstrates two approaches for migrating PostgreSQL databases to Amazon RDS:
**1.Manual Migration (Without DMS):**
Installed PostgreSQL locally and created a sample database (empdb).
Set up an Amazon RDS PostgreSQL instance with the same database name.
Connected both local and RDS databases using PgAdmin.
Performed backup and restore from local PostgreSQL to RDS.
Verified schema and data integrity post‑migration.
**2.Automated Migration (With AWS DMS):**
Created two RDS PostgreSQL instances: source-db and target-db.
Configured AWS Database Migration Service (DMS) with a Replication Instance.
Defined Source Endpoint and Target Endpoint with SSL enabled.
Created a Migration Task to replicate schema and data securely.
Monitored migration progress and validated data consistency using DMS metrics.
**AWS Services Used:**
Amazon RDS (PostgreSQL) – Managed relational database service.
AWS Database Migration Service (DMS) – Automated migration and replication.
Amazon S3 – Backup storage (optional).
PgAdmin – Database management tool.
SSL Encryption – Secure data transfer.
**Outcome:**
Successfully migrated PostgreSQL databases to AWS RDS using both manual and automated methods.
Automated migration with DMS reduced downtime and manual effort.
Demonstrated secure, reliable, and scalable cloud database migration architecture.
