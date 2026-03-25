# 1️⃣ PostgreSQL Installation
# ------------------------------------

Install PostgreSQL:

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

Start PostgreSQL:

```bash
sudo systemctl status postgresql
```

Login:

```bash
sudo -i -u postgres
psql
```

---

## ✔ Create Database and Schema

```sql
CREATE DATABASE mydb;
\c mydb;

CREATE SCHEMA school;

CREATE TABLE school.students(
    id INT PRIMARY KEY,
    name VARCHAR(20),
    age INT
);
```

---
