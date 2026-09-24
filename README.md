# AWS-Database-Migration

## 📌 Overview

This project demonstrates how to migrate a **PostgreSQL database to Amazon RDS** using two different approaches:

1. **Manual Migration** using backup and restore
2. **Automated Migration** using **AWS Database Migration Service (DMS)**

The project also covers RDS, DMS replication instances, source and target endpoints, SSL encryption, migration tasks, and data validation.

---

# 🔄 Migration Flow

### Manual Migration

```text
Local PostgreSQL
       ↓
Create Database
       ↓
Backup Database
       ↓
Amazon RDS PostgreSQL
       ↓
Restore Database
       ↓
Verify Data
```

### AWS DMS Migration

```text
Source PostgreSQL
       ↓
Source Endpoint
       ↓
DMS Replication Instance
       ↓
Target Endpoint
       ↓
RDS PostgreSQL
       ↓
Verify Data
```

---

# 1. Manual Migration

## Step 1: Install PostgreSQL

Install PostgreSQL on the local machine.

Check the installation:

```bash
psql --version
```

---

## Step 2: Create Database

Create a sample database:

```sql
CREATE DATABASE empdb;
```

Connect to the database:

```sql
\c empdb
```

Create a sample table:

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(100)
);
```

Insert sample data:

```sql
INSERT INTO employees (name, department)
VALUES
('Ajay', 'DevOps'),
('Rahul', 'Development'),
('Priya', 'Testing');
```

Check the data:

```sql
SELECT * FROM employees;
```

---

# 2. Create Amazon RDS PostgreSQL

Create an RDS PostgreSQL instance.

```text
Engine        → PostgreSQL
DB Identifier → target-db
Port          → 5432
Username      → postgres
Password      → <password>
```

Wait until the RDS instance status becomes:

```text
Available
```

---

# 3. Connect RDS Using pgAdmin

Open pgAdmin and configure the RDS connection:

```text
Host     → RDS Endpoint
Port     → 5432
Username → RDS Username
Password → RDS Password
```

Test the connection.

---

# 4. Backup PostgreSQL Database

Create a backup of the local PostgreSQL database:

```bash
pg_dump -U postgres -d empdb -F c -f empdb.backup
```

The backup contains the database structure and data.

---

# 5. Restore Database to RDS

Restore the backup into RDS:

```bash
pg_restore \
-h <RDS-ENDPOINT> \
-U <RDS-USERNAME> \
-d empdb \
empdb.backup
```

Enter the RDS password when prompted.

---

# 6. Verify Manual Migration

Connect to RDS:

```sql
\c empdb
```

List tables:

```sql
\dt
```

Check data:

```sql
SELECT * FROM employees;
```

Compare the source and target databases.

```text
Source PostgreSQL
       ↓
Tables + Data
       ↓
RDS PostgreSQL
       ↓
Tables + Data
```

If the structure and records match, the manual migration is successful.

---

# 7. Automated Migration Using AWS DMS

AWS Database Migration Service (DMS) is used to migrate and replicate data between databases.

### DMS Architecture

```text
Source PostgreSQL
       ↓
DMS Source Endpoint
       ↓
DMS Replication Instance
       ↓
DMS Target Endpoint
       ↓
Target RDS PostgreSQL
```

---

# 8. Create Source RDS

Create a PostgreSQL RDS instance as the source database.

```text
DB Identifier → source-db
Engine        → PostgreSQL
Port          → 5432
```

Create the required database, tables, and sample data.

---

# 9. Create Target RDS

Create another PostgreSQL RDS instance as the target database.

```text
DB Identifier → target-db
Engine        → PostgreSQL
Port          → 5432
```

The migrated data will be stored in this database.

---

# 10. Configure Security Groups

Security groups control communication between the source, DMS, and target databases.

```text
Source RDS
    ↕
DMS Replication Instance
    ↕
Target RDS
```

PostgreSQL uses:

```text
Port → 5432
```

Database access should be restricted to the required sources.

---

# 11. Create DMS Replication Instance

Go to:

```text
AWS Console
   ↓
AWS DMS
   ↓
Replication instances
   ↓
Create replication instance
```

Example:

```text
Name → postgres-migration
```

Wait until:

```text
Status → Available
```

The replication instance performs the migration between the source and target databases.

---

# 12. Create Source Endpoint

Create the DMS source endpoint:

```text
Endpoint Type → Source
Engine        → PostgreSQL
Server        → Source RDS Endpoint
Port          → 5432
Database      → empdb
Username      → postgres
Password      → <password>
```

Test the connection.

```text
Connection successful
```

---

# 13. Create Target Endpoint

Create the DMS target endpoint:

```text
Endpoint Type → Target
Engine        → PostgreSQL
Server        → Target RDS Endpoint
Port          → 5432
Database      → empdb
Username      → postgres
Password      → <password>
```

Test the connection.

```text
Connection successful
```

---

# 14. SSL Encryption

SSL/TLS can be configured to secure database communication.

```text
Source PostgreSQL
       ↓
    SSL/TLS
       ↓
AWS DMS
       ↓
    SSL/TLS
       ↓
Target RDS PostgreSQL
```

This helps protect database traffic during migration.

---

# 15. Create DMS Migration Task

Go to:

```text
AWS DMS
   ↓
Database migration tasks
   ↓
Create task
```

Configure:

```text
Task Name            → postgres-migration
Replication Instance → postgres-migration
Source Endpoint      → source-db
Target Endpoint      → target-db
```

---

# 16. Migration Types

## Full Load

Full Load copies existing data from the source database to the target database.

```text
Source Database
       ↓
   Full Load
       ↓
Target Database
```

## Full Load + CDC

Full Load + CDC copies the existing data and then captures ongoing changes.

```text
Existing Data
      ↓
 Full Load
      ↓
Change Data Capture
      ↓
Target Database
```

**CDC = Change Data Capture**

---

# 17. Table Mapping

Table mappings define which tables should be migrated.

Example:

```text
Schema → public
Table  → %
Action → Include
```

For a specific table:

```text
Schema → public
Table  → employees
Action → Include
```

---

# 18. Start Migration

Start the DMS migration task.

Typical status:

```text
Starting
   ↓
Running
   ↓
Load Complete
```

Monitor:

* Migration status
* Tables loaded
* Rows loaded
* Errors
* DMS metrics

---

# 19. Verify Target Database

Connect to the target RDS PostgreSQL database.

List tables:

```sql
\dt
```

Check data:

```sql
SELECT * FROM employees;
```

Check row count:

```sql
SELECT COUNT(*) FROM employees;
```

Compare the source and target row counts.

```text
Source Row Count
       ↓
Target Row Count
       ↓
Compare
       ↓
Data Validation
```

---

# 🛠️ AWS Services & Tools Used

| Service / Tool  | Purpose                            |
| --------------- | ---------------------------------- |
| Amazon RDS      | Managed PostgreSQL database        |
| AWS DMS         | Database migration and replication |
| pgAdmin         | PostgreSQL database management     |
| PostgreSQL      | Database engine                    |
| Security Groups | Network access control             |
| SSL/TLS         | Secure database communication      |
| Amazon S3       | Optional backup/storage            |

---

# 📚 Key Concepts Learned

* PostgreSQL
* Amazon RDS
* AWS Database Migration Service
* DMS Replication Instance
* Source Endpoint
* Target Endpoint
* DMS Migration Task
* Full Load
* Change Data Capture (CDC)
* Table Mapping
* PostgreSQL Backup
* PostgreSQL Restore
* `pg_dump`
* `pg_restore`
* Security Groups
* SSL/TLS
* Database Validation
* Source and Target Database Comparison

---

# 🎯 Final Architecture

```text
                Source
          PostgreSQL Database
                  │
                  ▼
           DMS Source Endpoint
                  │
                  ▼
        DMS Replication Instance
                  │
                  ▼
           DMS Target Endpoint
                  │
                  ▼
           Amazon RDS
          PostgreSQL Target
                  │
                  ▼
            Data Validation
```

# 🚀 Key Takeaways

Through this hands-on project, I learned how to migrate PostgreSQL databases to AWS RDS using both **manual migration** and **AWS DMS**.

I practiced database backup and restore, RDS configuration, DMS replication instances, source and target endpoints, SSL-secured connectivity, migration tasks, Full Load, CDC, and data validation.

This project helped me understand how database migration can be implemented as part of a real-world **AWS and DevOps workflow**.
