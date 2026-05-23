# LAB 03 — Hive & Sqoop Integration

## Objective

This lab demonstrates how to:

- Connect MariaDB/MySQL with Hadoop
- Import relational database tables into Hive using Sqoop
- Create ORC tables in Hive
- Clean and transform data for analytics
- Verify imported data using Hive queries

---

# Prerequisites

Before starting this lab:

- AWS EMR Cluster must be running
- Hive installed
- Sqoop installed
- MariaDB/MySQL installed
- SSH access available

---

# Architecture Overview

```text
MariaDB/MySQL
       │
       ▼
     Sqoop
       │
       ▼
      Hive
       │
       ▼
   ORC Tables
```

---

# Step 1 — Verify MariaDB Service

Check MariaDB status:

```bash
sudo systemctl status mariadb
```

Expected result:

```text
active (running)
```

---

# Step 2 — Connect to MariaDB

```bash
mysql -u root -p -h [PRIVATE-IP]
```

Example:

```bash
mysql -u root -p -h 172.31.86.115
```

---

# Step 3 — Create Northwind Database

Inside MariaDB:

```sql
CREATE DATABASE northwind;
```

Use database:

```sql
USE northwind;
```

---

# Step 4 — Create Sample Tables

## Orders Table

```sql
CREATE TABLE orders (
  orderid INT,
  orderdate DATE
);
```

Insert sample data:

```sql
INSERT INTO orders VALUES
(1,'2026-01-01'),
(2,'2026-02-01'),
(3,'2026-03-01');
```

---

## Products Table

```sql
CREATE TABLE products (
  productid INT,
  productname VARCHAR(100),
  unitprice DOUBLE,
  unitsinstock INT,
  reorderlevel INT
);
```

Insert sample data:

```sql
INSERT INTO products VALUES
(1,'Coffee',20,100,10),
(2,'Tea',15,200,20),
(3,'Milk',30,150,15);
```

---

## OrderDetails Table

```sql
CREATE TABLE orderdetails (
  orderid INT,
  productid INT,
  unitprice DOUBLE,
  quantity INT,
  discount DOUBLE
);
```

Insert sample data:

```sql
INSERT INTO orderdetails VALUES
(1,1,20,2,0),
(2,2,15,3,0.1),
(3,3,30,1,0);
```

---

# Step 5 — Verify Tables

```sql
SHOW TABLES;
```

Expected result:

```text
orders
orderdetails
products
```

Exit MariaDB:

```sql
exit;
```

---

# Step 6 — Import Orders Table into Hive

```bash
sqoop import \
--driver com.mysql.jdbc.Driver \
--connect jdbc:mysql://172.31.86.115:3306/northwind \
--username root \
--password password \
--table orders \
--hive-import \
--create-hive-table \
--hive-table orders \
-m 1
```

---

# Step 7 — Import OrderDetails Table

```bash
sqoop import \
--driver com.mysql.jdbc.Driver \
--connect jdbc:mysql://172.31.86.115:3306/northwind \
--username root \
--password password \
--table orderdetails \
--hive-import \
--create-hive-table \
--hive-table orderdetails \
-m 1
```

---

# Step 8 — Import Products Table

```bash
sqoop import \
--driver com.mysql.jdbc.Driver \
--connect jdbc:mysql://172.31.86.115:3306/northwind \
--username root \
--password password \
--table products \
--hive-import \
--create-hive-table \
--hive-table products \
-m 1
```

---

# Step 9 — Open Hive

```bash
hive
```

---

# Step 10 — Verify Imported Tables

```sql
SHOW TABLES;
```

Expected result:

```text
orders
orderdetails
products
```

---

# Step 11 — Query Data

## Orders

```sql
SELECT * FROM orders;
```

---

## Products

```sql
SELECT * FROM products;
```

---

# Step 12 — Convert Tables to ORC Format

## Orders ORC

```sql
CREATE TABLE orders_orc
STORED AS ORC
AS SELECT * FROM orders;
```

---

## OrderDetails ORC

```sql
CREATE TABLE orderdetails_orc
STORED AS ORC
AS SELECT * FROM orderdetails;
```

---

## Products ORC

```sql
CREATE TABLE products_orc
STORED AS ORC
AS SELECT * FROM products;
```

---

# Step 13 — Verify ORC Format

```sql
DESCRIBE FORMATTED products_orc;
```

Expected output should contain:

```text
OrcInputFormat
```

---

# Step 14 — Data Cleaning

Convert orderdate into TIMESTAMP format:

```sql
CREATE TABLE orders_cleaned AS
SELECT
  orderid,
  CAST(orderdate AS TIMESTAMP) AS orderdate
FROM orders_orc;
```

---

# Step 15 — Verify Cleaned Data

```sql
SELECT * FROM orders_cleaned;
```

---

# Common Errors

## Error: Could not load db driver class

Cause:
- MySQL connector not installed

Solution:

```bash
ls /usr/lib/sqoop/lib/ | grep mysql
```

Expected:

```text
mysql-connector-j-8.0.33.jar
```

---

## Error: Connection refused

Cause:
- MariaDB service not running

Solution:

```bash
sudo systemctl restart mariadb
```

---

## Error: Table doesn't exist

Cause:
- Wrong table name
- Uppercase/lowercase mismatch

Solution:

```sql
SHOW TABLES;
```

Use exact table name.

---

## Error: SHOW TABLE;

Incorrect syntax:

```sql
SHOW TABLE;
```

Correct syntax:

```sql
SHOW TABLES;
```

---

# Screenshots to Capture

- MariaDB tables
- Sqoop import success
- Hive SHOW TABLES
- ORC verification
- orders_cleaned output

---

# Expected Result

Successfully:

- Imported MariaDB tables into Hive
- Created ORC tables
- Cleaned timestamp data
- Verified imported datasets

---

# Technologies Used

- AWS EMR
- Hadoop
- Hive
- Sqoop
- MariaDB/MySQL

---

# Author

Vikhom Manpiriya
