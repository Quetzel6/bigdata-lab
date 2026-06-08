# LAB08 — ODBC/JDBC & Apache Kafka

## Objective

This lab introduces database connectivity technologies and Apache Kafka for 
real-time event streaming.

Students will learn how to:

* Create Hive tables
* Import data using Sqoop
* Configure JDBC and ODBC connectivity
* Connect Hive to external tools
* Understand Kafka architecture
* Create Kafka topics
* Use producers and consumers
* Understand consumer groups and offsets
* Build Kafka applications using Python
* Configure Kafka authentication using SASL

---

# Technologies Used

* AWS EMR
* Hive
* Sqoop
* MySQL JDBC
* ODBC
* DBeaver
* Power BI
* Apache Kafka
* Python
* SASL Authentication

---

# Part 1 — Environment Preparation

## Step 1.1 Create EMR Cluster

Applications:

* Core Hadoop
* Sqoop
* Oozie

---

# Part 2 — Hive Product Table Setup

## Step 2.1 Create Products Table

```sql
CREATE EXTERNAL TABLE products (
...
);
```

## Step 2.2 Verify Table

```sql
SHOW TABLES;
DESC products;
```

---

# Part 3 — Secure Password File

## Step 3.1 Create Password File

```bash
echo -n "clusterkit2001" > .password
```

## Step 3.2 Upload to HDFS

```bash
hadoop fs -put .password
```

## Step 3.3 Secure Permissions

```bash
hadoop fs -chmod 600 .password
```

---

# Part 4 — Install MySQL JDBC Driver

## Step 4.1 Download Driver

```bash
wget ...
```

## Step 4.2 Extract Package

```bash
tar xvf ...
```

## Step 4.3 Copy Driver

```bash
cp ...
```

## Step 4.4 Configure Sqoop JDBC

```bash
ln -sf ...
```

## Step 4.5 Update Oozie Sharelib

```bash
sudo -u oozie hadoop fs -put ...
```

## Step 4.6 Restart Oozie

```bash
systemctl restart oozie
```

---

# Part 5 — Sqoop Import

## Step 5.1 Import Products Table

```bash
sqoop import ...
```

## Step 5.2 Verify Hive Data

```sql
SELECT * FROM products LIMIT 10;
```

---

# Part 6 — JDBC Concepts

## What is JDBC

Java Database Connectivity (JDBC) is a standard API that enables Java 
applications to communicate with databases.

### JDBC Architecture

Application
↓
JDBC Driver
↓
Database

### Benefits

* Database independence
* Standard API
* Secure connectivity

---

# Part 7 — ODBC Concepts

## What is ODBC

Open Database Connectivity (ODBC) allows applications to connect to different 
databases using a common interface.

### ODBC Architecture

Application
↓
ODBC Driver
↓
Database

### Benefits

* Platform independence
* Tool integration
* Business intelligence support

---

# Part 8 — DBeaver and Power BI Connectivity

## DBeaver

### Install DBeaver

### Configure JDBC Connection

Host:
Port:
Database:

### Verify Connection

---

## Power BI

### Install Hive ODBC Driver

### Configure DSN

### Connect via ODBC

---

## Microsoft Excel

### Data → Get Data → ODBC

---

# Part 9 — Apache Kafka Fundamentals

## What is Apache Kafka

Apache Kafka is a distributed event streaming platform.

---

## Kafka Components

### Producer

Sends messages.

### Consumer

Reads messages.

### Topic

Stores messages.

### Partition

Enables parallel processing.

### Broker

Kafka server.

---

# Part 10 — Consumer Groups and Offsets

## Offset

An offset is a unique position of a message inside a partition.

Example:

Offset 0
Offset 1
Offset 2

---

## Group ID

A consumer group shares work across consumers.

---

## Benefits

* Fault tolerance
* Scalability
* Recovery after failure

---

# Part 11 — Kafka Installation

## Step 11.1 Download Kafka

```bash
wget ...
```

## Step 11.2 Extract Package

```bash
tar xvf ...
```

## Step 11.3 Configure Java 21

```bash
sudo alternatives --config java
```

## Step 11.4 Format Kafka Storage

```bash
kafka-storage.sh ...
```

## Step 11.5 Start Kafka

```bash
kafka-server-start.sh ...
```

---

# Part 12 — Kafka Topics

## Create Topic

```bash
kafka-topics.sh --create ...
```

## List Topics

```bash
kafka-topics.sh --list ...
```

---

# Part 13 — Producer and Consumer

## Producer

```bash
kafka-console-producer.sh ...
```

## Consumer

```bash
kafka-console-consumer.sh ...
```

## Consumer Group

```bash
kafka-console-consumer.sh --group ...
```

---

# Part 14 — Python Kafka Consumer

## Install Library

```bash
pip install kafka-python
```

## Create Consumer Program

```python
from kafka import KafkaConsumer
...
```

## Execute

```bash
python kafka-python.py
```

---

# Part 15 — Kafka Security (SASL)

## SASL Overview

Simple Authentication and Security Layer (SASL) provides authentication for 
Kafka clients.

---

## Configure Server

### Edit server.properties

```properties
listeners=SASL_PLAINTEXT://...
```

### Enable PLAIN Mechanism

```properties
sasl.enabled.mechanisms=PLAIN
```

---

## Create JAAS Configuration

```text
KafkaServer {
...
};
```

---

## Restart Kafka

```bash
kafka-server-stop.sh
kafka-server-start.sh ...
```

---

## Configure Client Authentication

```properties
security.protocol=SASL_PLAINTEXT
```

---

## Secure Producer

```bash
kafka-console-producer.sh ...
```

## Secure Consumer

```bash
kafka-console-consumer.sh ...
```

---

# Part 16 — Screenshots

## Producer Output

![Producer Output](images/producer-output.png)

---

## Consumer Output

![Consumer Output](images/consumer-output.png)

---

## Analytics Output

![Analytics Output](images/analytics-output.png)

---

# Part 17 — Troubleshooting

## JDBC Driver Not Found

### Solution

Reinstall MySQL JDBC driver.

---

## Sqoop Import Failed

### Solution

Verify credentials and JDBC configuration.

---

## Kafka Server Not Starting

### Solution

Check Java version.

```bash
java -version
```

---

## Consumer Not Receiving Messages

### Solution

Verify:

* Topic name
* Bootstrap server
* Group ID

---

## SASL Authentication Failed

### Solution

Verify:

* JAAS configuration
* Username
* Password

---

# Part 18 — Conclusion

In this lab, we learned how to:

* Connect Hive using JDBC and ODBC
* Import data using Sqoop
* Configure DBeaver and Power BI
* Understand Kafka architecture
* Create topics and stream data
* Use consumer groups and offsets
* Build Kafka consumers using Python
* Secure Kafka using SASL

These technologies form the foundation of modern data engineering pipelines 
and real-time event streaming systems.

---

# Author

Vikhom Manpiriya

Student ID: 66102010185

