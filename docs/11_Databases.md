# Database Cheat Sheet

- [SQL Database](#1-sql-databases-mysql-postgresql-mariadb)
- [NoSQL Database](#2-nosql-databases)
- [Database Automation](#3-database-automation)
- [Deploy MongoDB with Docker](#4-deploying-mongodb-with-docker-compose)

---

!!! note ""
    Databases are essential for CI/CD pipelines, monitoring, logging, and cloud automation. DevOps engineers interact with databases to store configurations, manage infrastructure state, and automate deployments. This guide covers SQL, NoSQL, and cloud databases with relevant DevOps commands and use cases.

---

## Database Automation for DevOps

!!! note "Why Automate Databases in DevOps?"
    - Eliminate manual work in database provisioning, backup, and monitoring
    - Ensure consistency across environments (dev, staging, production)
    - Reduce downtime with automated backups and performance monitoring
    - Enable CI/CD pipelines to manage database migrations

## 1. SQL Databases (MySQL, PostgreSQL, MariaDB)

### Database Management

```sql title="List databases"
SHOW DATABASES;
```
```sql title="Create a database"
CREATE DATABASE db_name;
```
```sql title="Delete a database"
DROP DATABASE db_name;
```
```sql title="Select a database"
USE db_name;
```

### User Management

```sql
CREATE USER 'devops'@'localhost' IDENTIFIED BY 'password';
```
```sql
GRANT ALL PRIVILEGES ON db_name.\* TO 'devops'@'localhost';
```

### Table & Data Operations

```sql title="List tables"
SHOW TABLES;
```
```sql
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
```
```sql
SELECT * FROM users;
```

### Backup & Restore

```bash title="Backup MySQL"
mysqldump -u root -p db_name > backup.sql
```
```bash title="Restore MySQL"
mysql -u root -p db_name < backup.sql
```

## 2. NoSQL Databases

### MongoDB

```sql title="List databases"
show dbs;
```
```sql title="Select a database"
use mydb;
```
```sql title="Create a collection"
db.createCollection("users");
```
```sql title="Insert data"
db.users.insertOne({name: "Alice"});
```
```sql title="Backup"
mongodump --out /backup/
```

### Redis

```bash title="Store a key"
SET key "value";
```
```bash title="Retrieve value"
GET key;
```
```bash title="Delete key"
DEL key;
```

### Cassandra (CQL)

```sql title=""
CREATE KEYSPACE mykeyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
```
```sql title=""
CREATE TABLE users (id UUID PRIMARY KEY, name TEXT);
```

## 3. Database Automation

### Terraform for AWS RDS

```tf
resource "aws_db_instance" "default" { 
  identifier = "devops-db"
  engine = "mysql" 
  instance_class = "db.t3.micro" 
  allocated_storage = 20
}
```

### Docker Database Containers

```bash
docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 mysql
```
```bash
docker run -d --name postgres -e POSTGRES_PASSWORD=root -p 5432:5432 postgres
```

### Automating MySQL with Ansible

```yaml
- name: Install MySQL
  hosts: db_servers
  become: yes
  tasks:
  - name: Install MySQL
    apt: name=mysql-server state=present
  - name: Create Database
    mysql_db: name=devops_db state=present
```

### Jenkins Pipeline for Database Backup

```groovy
pipeline {
  agent any
  stages {
    stage('Backup') {
      steps { sh 'mysqldump -u root -p$MYSQL_ROOT_PASSWORD db > /backup/db.sql' }
    }
    stage('Restore') {
      steps { sh 'mysql -u root -p$MYSQL_ROOT_PASSWORD db < /backup/db.sql' }
    }
  }
}
```

### Database Monitoring (MySQL + Prometheus & Grafana)

```bash
wget https://github.com/prometheus/mysqld_exporter/releases/download/v0.14.0/mysql d_exporter.tar.gz
```

```bash
tar xvf mysqld_exporter.tar.gz && mv mysqld_exporter /usr/local/bin/
```

```bash
mysqld_exporter --config.my-cnf=/etc/.my.cnf &
```

## 4. Deploying MongoDB with Docker Compose

```yaml
version: '3.8'
services:
  mongo:
    image: mongo
    container_name: mongodb
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: DevOps@123
    ports:
    - "27017:27017"
```
