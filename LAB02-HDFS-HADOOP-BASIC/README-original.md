# LAB02 — HDFS & Hadoop Basic

## Objective

This lab introduces basic Hadoop Distributed File System (HDFS) operations 
on AWS EMR.

---

## Step 1 — Connect to EMR Cluster

```bash
ssh -i ~/Downloads/bigdata.pem hadoop@[EMR-PUBLIC-IP]
```

---

## Step 2 — Check Hadoop Version

```bash
hadoop version
```

![Hadoop Version](images/hadoop-version.png)

---

## Step 3 — Create Local File

```bash
nano RE_66102010185.csv
```

Example content:

```text
66102010185,Vikhom Manpiriya
```

---

## Step 4 — Create HDFS Directory

```bash
hadoop fs -mkdir input
```

---

## Step 5 — Upload File to HDFS

```bash
hadoop fs -put RE_66102010185.csv input
```

---

## Step 6 — Verify Uploaded File

```bash
hadoop fs -ls input
```

![HDFS File Listing](images/hdfs-file-list.png)

---

## Step 7 — Read File from HDFS

```bash
hadoop fs -cat input/RE_66102010185.csv
```

![HDFS CAT](images/hdfs-cat.png)

---

## Step 8 — Open HDFS Browser

Open browser:

```text
http://[EMR-PUBLIC-IP]:9870
```

Navigate to:

```text
Utilities → Browse the file system
```

Path:

```text
/user/hadoop/input
```

![HDFS Browser](images/hdfs-browser.png)

---

## Step 9 — Verify Replication Factor

Click the uploaded file and verify block replication information.

![Replication Detail](images/replication-detail.png)

---

## Conclusion

This lab demonstrates:
- Hadoop basic operations
- HDFS file management
- File replication
- HDFS Web UI verification

--

#Author 
Vikhom Manpiriya
