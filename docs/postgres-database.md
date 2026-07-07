# PostgreSQL Database Setup for Voting Application

This guide walks through creating a dedicated PostgreSQL database and application user for the Voting Application, storing the credentials in Google Secret Manager, and configuring the Kubernetes application.

---

# Prerequisites

* Cloud SQL for PostgreSQL instance is running
* Private IP connectivity to the Cloud SQL instance
* pgAdmin installed
* Google Cloud CLI (`gcloud`) authenticated
* Required IAM permissions to manage Cloud SQL and Secret Manager

---

# Step 1: Connect to PostgreSQL

Connect to your Cloud SQL instance using **pgAdmin** with the following connection details.

| Parameter | Value                                                    |
| --------- | -------------------------------------------------------- |
| Host      | `10.160.96.5` *(Replace with your Cloud SQL private IP)* |
| Port      | `5432`                                                   |
| Database  | `postgres`                                               |
| Username  | `postgres`                                               |
| Password  | `postgres123`                                            |

---

# Step 2: Remove Existing Database (Optional)

If the database already exists, terminate any active connections before dropping it.

```sql
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE datname = 'votingdb'
AND pid <> pg_backend_pid();
```

Drop the database if it exists.

```sql
DROP DATABASE IF EXISTS votingdb;
```

> **Note**
>
> Ensure all Kubernetes workloads (vote, result, and worker) are stopped before dropping the database to avoid active connections.

---

# Step 3: Create the Application User

Create a dedicated PostgreSQL login for the application.

```sql
CREATE ROLE votingapp
LOGIN
PASSWORD 'VotingApp@123';
```

Verify the role:

```sql
SELECT rolname, rolcanlogin
FROM pg_roles
WHERE rolname = 'votingapp';
```

Expected output:

| rolname   | rolcanlogin |
| --------- | ----------- |
| votingapp | t           |

---

# Step 4: Grant Temporary Database Creation Permission

Temporarily allow the application user to create the database.

```sql
ALTER ROLE votingapp CREATEDB;
```

> **Why is this required?**
>
> Cloud SQL for PostgreSQL 15+ uses the special `pg_database_owner` role for ownership of the `public` schema. Creating the database as `votingapp` ensures the application user owns the database and can create tables without additional permission issues.

---

# Step 5: Create the Application Database

Disconnect from the **postgres** connection.

Create a new pgAdmin connection using:

| Parameter | Value           |
| --------- | --------------- |
| Host      | `10.160.96.5`   |
| Port      | `5432`          |
| Database  | `postgres`      |
| Username  | `votingapp`     |
| Password  | `VotingApp@123` |

While connected as **votingapp**, create the database:

```sql
CREATE DATABASE votingdb;
```

Verify the owner:

```sql
SELECT datname,
       pg_get_userbyid(datdba) AS owner
FROM pg_database
WHERE datname = 'votingdb';
```

Expected output:

| Database | Owner     |
| -------- | --------- |
| votingdb | votingapp |

---

# Step 6: Verify Database Permissions

Reconnect as **votingapp** using the **votingdb** database.

Verify that the application user can create tables.

```sql
CREATE TABLE public.permission_test (
    id INT PRIMARY KEY
);
```

Drop the test table.

```sql
DROP TABLE public.permission_test;
```

If both commands succeed, the database permissions are configured correctly.

---

# Step 7: (Optional) Remove Temporary CREATEDB Permission

Reconnect as **postgres** and execute:

```sql
ALTER ROLE votingapp NOCREATEDB;
```

> **Note**
>
> Some Cloud SQL environments restrict modifying role attributes after creation. If this command returns a permission error, it can be safely skipped in development environments.

---

# Step 8: Store Credentials in Google Secret Manager

Create the required secrets.

```bash
echo -n "votingapp" | gcloud secrets create votingapp-username --data-file=-

echo -n "VotingApp@123" | gcloud secrets create votingapp-password --data-file=-

echo -n "votingdb" | gcloud secrets create postgres-db --data-file=-

echo -n "10.160.96.5" | gcloud secrets create postgres-host --data-file=-
```

If the secrets already exist, create new versions instead.

```bash
echo -n "votingapp" | gcloud secrets versions add votingapp-username --data-file=-

echo -n "VotingApp@123" | gcloud secrets versions add votingapp-password --data-file=-

echo -n "votingdb" | gcloud secrets versions add postgres-db --data-file=-

echo -n "10.160.96.5" | gcloud secrets versions add postgres-host --data-file=-
```

---

# Step 9: Configure the Kubernetes Application

Configure the application with the following PostgreSQL environment variables.

| Environment Variable | Value           |
| -------------------- | --------------- |
| `POSTGRES_HOST`      | `10.160.96.5`   |
| `POSTGRES_PORT`      | `5432`          |
| `POSTGRES_DB`        | `votingdb`      |
| `POSTGRES_USER`      | `votingapp`     |
| `POSTGRES_PASSWORD`  | `VotingApp@123` |

These values can be injected into Kubernetes workloads using **External Secrets** integrated with **Google Secret Manager**.

---

# Verification

Before deploying the application through ArgoCD, verify:

* `votingdb` exists.
* `votingapp` is the database owner.
* `votingapp` can successfully create and drop tables.
* Google Secret Manager contains the latest database credentials.
* External Secrets has synchronized the Kubernetes secret successfully.

Once verified, sync the ArgoCD applications (`vote`, `result`, and `worker`) and confirm that all pods start successfully without PostgreSQL permission errors.
