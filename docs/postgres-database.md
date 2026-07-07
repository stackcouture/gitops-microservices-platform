# PostgreSQL Database Setup for Voting Application

This guide walks through creating a dedicated PostgreSQL database and application user for the Voting Application, storing the credentials in Google Secret Manager, and configuring the Kubernetes application.

---

## Prerequisites

- Cloud SQL for PostgreSQL instance is running
- Private IP connectivity to the Cloud SQL instance
- pgAdmin installed
- Google Cloud CLI (`gcloud`) authenticated
- Required IAM permissions to manage Cloud SQL and Secret Manager

---

# Step 1: Connect to PostgreSQL

Connect to your Cloud SQL instance using **pgAdmin** with the following connection details.

| Parameter | Value |
|-----------|-------|
| Host | `10.160.96.5` *(Replace with your Cloud SQL private IP)* |
| Port | `5432` |
| Database | `postgres` |
| Username | `postgres` |
| Password | `postgres123` |

---

# Step 2: Remove Existing Database and User (Optional)

If the database or application user already exists, remove them before creating fresh resources.

```sql
DROP DATABASE IF EXISTS votingdb;
DROP ROLE IF EXISTS votingapp;
```

### If the database is currently in use

Terminate all active connections:

```sql
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE datname = 'votingdb'
AND pid <> pg_backend_pid();
```

Then drop the database:

```sql
DROP DATABASE votingdb;
```

---

# Step 3: Create the Application User

Create a dedicated PostgreSQL login for the application.

```sql
CREATE ROLE votingapp
LOGIN
PASSWORD 'VotingApp@123';
```

### Verify

```sql
SELECT rolname, rolcanlogin
FROM pg_roles
WHERE rolname = 'votingapp';
```

Expected output:

| rolname | rolcanlogin |
|----------|-------------|
| votingapp | t |

---

# Step 4: Create the Application Database

Create a dedicated database for the Voting Application.

```sql
CREATE DATABASE votingdb;
```

### Verify

```sql
SELECT datname
FROM pg_database;
```

Expected output:

```
postgres
votingdb
```

---

# Step 5: Grant Database Permissions

Connect to the newly created **votingdb** database in pgAdmin and execute the following SQL statements.

Grant database access:

```sql
GRANT CONNECT ON DATABASE votingdb TO votingapp;
```

Grant schema permissions:

```sql
GRANT USAGE, CREATE ON SCHEMA public TO votingapp;
```

Grant table permissions:

```sql
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO votingapp;
```

Grant sequence permissions:

```sql
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO votingapp;
```

Grant default privileges for future tables:

```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT ALL ON TABLES TO votingapp;
```

Grant default privileges for future sequences:

```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT ALL ON SEQUENCES TO votingapp;
```

> **Note**
>
> Cloud SQL may return a permission error for the `ALTER DEFAULT PRIVILEGES` commands. This can be safely ignored, as the previous `GRANT` statements are sufficient for most application workloads.

---

# Step 6: Reset the Application Password (Optional)

If required, reset the application user's password.

```sql
ALTER ROLE votingapp
WITH PASSWORD 'VotingApp@123';
```

---

# Step 7: Verify the Application User

Create a new server connection in **pgAdmin** using the application credentials.

## General

| Field | Value |
|--------|-------|
| Name | Voting App |

## Connection

| Field | Value |
|--------|-------|
| Host | `10.160.96.5` *(Replace with your Cloud SQL private IP)* |
| Port | `5432` |
| Maintenance Database | `votingdb` |
| Username | `votingapp` |
| Password | `VotingApp@123` |

If the connection succeeds, the database configuration is complete.

---

# Step 8: Store Credentials in Google Secret Manager

Create the required secrets.

```bash
echo -n "votingapp" | gcloud secrets create votingapp-username --data-file=-

echo -n "VotingApp@123" | gcloud secrets create votingapp-password --data-file=-

echo -n "votingdb" | gcloud secrets create postgres-db --data-file=-

echo -n "10.160.96.17" | gcloud secrets create postgres-host --data-file=-
```

If the secrets already exist, create new versions instead.

```bash
echo -n "votingapp" | gcloud secrets versions add votingapp-username --data-file=-

echo -n "VotingApp@123" | gcloud secrets versions add votingapp-password --data-file=-
```

---

# Step 9: Configure the Kubernetes Application

Configure the application with the following PostgreSQL environment variables.

| Environment Variable | Value |
|----------------------|-------|
| `POSTGRES_HOST` | `10.160.96.17` |
| `POSTGRES_PORT` | `5432` |
| `POSTGRES_DB` | `votingdb` |
| `POSTGRES_USER` | `votingapp` |
| `POSTGRES_PASSWORD` | `VotingApp@123` |

These values can be injected into Kubernetes workloads using **External Secrets** integrated with **Google Secret Manager**.