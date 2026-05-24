# LAB 02 — HDFS & Hadoop Basic Commands

## Objective

This lab introduces Hadoop Distributed File System (HDFS) basic operations 
including:

- Creating files
- Uploading files to HDFS
- Viewing file contents
- Managing directories
- Checking replication factors

---

# Prerequisites

- AWS EMR Cluster Running
- SSH Access to EMR Master Node
- Hadoop Installed

---

# Step 1 — Connect to EMR

```bash
ssh -i [private-key.pem] hadoop@[EMR-PUBLIC-IP]
```

Example:

```bash
ssh -i bigdata.pem hadoop@18.210.11.70
```

---

# Step 2 — Check Hadoop Installation

```bash
hadoop version
```

Expected output:

```text
Hadoop 3.x.x
```

---

# Step 3 — Create Local File

Create a CSV file containing your name.

```bash
nano RE_65102010100.csv
```

Example content:

```text
65102010100,Vikhom Manpiriya
```

Save file.

---

# Step 4 — Create HDFS Directory

```bash
hdfs dfs -mkdir -p /user/hadoop
```

---

# Step 5 — Upload File to HDFS

```bash
hdfs dfs -put RE_65102010100.csv /user/hadoop/
```

---

# Step 6 — Verify Uploaded File

List files:

```bash
hdfs dfs -ls /user/hadoop/
```

---

# Step 7 — Display File Content

```bash
hdfs dfs -cat /user/hadoop/RE_65102010100.csv
```

Expected output:

```text
65102010100,Vikhom Manpiriya
```

---

# Step 8 — Set Replication Factor

```bash
hdfs dfs -setrep -w 2 /user/hadoop/RE_65102010100.csv
```

---

# Step 9 — Verify Replication

```bash
hdfs fsck /user/hadoop/RE_65102010100.csv -files -blocks
```

Expected output should contain:

```text
Replication factor: 2
```

---

# Step 10 — Open NameNode Web UI

Open browser:

```text
http://[EMR-MASTER-IP]:9870
```

Navigate to:

```text
Utilities → Browse the file system
```

Verify:
- File exists
- Replication factor = 2

---

# Common HDFS Commands

## Create Directory

```bash
hdfs dfs -mkdir /data
```

---

## Remove File

```bash
hdfs dfs -rm /user/hadoop/file.csv
```

---

## Remove Directory

```bash
hdfs dfs -rm -r /data
```

---

## Copy From Local to HDFS

```bash
hdfs dfs -put localfile.txt /data/
```

---

## Copy From HDFS to Local

```bash
hdfs dfs -get /data/file.txt
```

---

# Common Errors

## Error: File exists

Solution:

```bash
hdfs dfs -rm /user/hadoop/file.csv
```

Then upload again.

---

## Error: Permission denied

Ensure current user has permission to access HDFS directory.

---

# Expected Result

- File uploaded to HDFS
- Replication factor set to 2
- File visible in NameNode Web UI

---

# Screenshots to Capture

- Hadoop version
- HDFS file listing
- HDFS file content
- Replication factor
- NameNode Web UI

---

# Author

Vikhom Manpiriya
