# LAB10 — Hadoop Installation from Scratch

## Objective

This lab demonstrates how to install and configure Apache Hadoop manually 
on Linux systems.

Students learn:
- Java installation
- Hadoop installation
- Hadoop environment configuration
- HDFS setup
- YARN setup
- NameNode formatting
- Hadoop service management

---

# Technologies Used

- Linux
- Apache Hadoop
- HDFS
- YARN
- Java OpenJDK

---

# Hadoop Architecture

```text
Client
   │
   ▼
NameNode
   │
   ▼
DataNode
```

---

# Part 1 — Install Java

## Install OpenJDK

```bash
sudo yum install java-1.8.0-openjdk -y
```

---

## Verify Java

```bash
java -version
```

---

# Part 2 — Download Hadoop

## Download Hadoop Package

```bash
wget 
https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
```

---

## Extract Hadoop

```bash
tar -xvzf hadoop-3.3.6.tar.gz
```

---

# Part 3 — Configure Environment Variables

Edit:

```bash
nano ~/.bashrc
```

Add:

```bash
export HADOOP_HOME=~/hadoop-3.3.6
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
```

Reload:

```bash
source ~/.bashrc
```

---

# Part 4 — Configure Hadoop XML Files

Configuration files:
- core-site.xml
- hdfs-site.xml
- mapred-site.xml
- yarn-site.xml

---

# Part 5 — Format NameNode

```bash
hdfs namenode -format
```

---

# Part 6 — Start Hadoop Services

## Start HDFS

```bash
start-dfs.sh
```

---

## Start YARN

```bash
start-yarn.sh
```

---

# Part 7 — Verify Services

```bash
jps
```

Expected services:
- NameNode
- DataNode
- SecondaryNameNode
- ResourceManager
- NodeManager

---

# HDFS Web UI

Open browser:

```text
http://localhost:9870
```

---

# YARN Web UI

```text
http://localhost:8088
```

---

# Common Troubleshooting

| Problem | Solution |
|---|---|
| JAVA_HOME not found | Configure JAVA_HOME |
| Permission denied | Fix file permissions |
| NameNode failed | Re-format NameNode |
| Port already used | Kill existing process |

---

# Screenshots

## Hadoop Services

```md
![JPS](images/jps.png)
```

---

## HDFS Web UI

```md
![HDFS UI](images/hdfs-ui.png)
```

---

## YARN Web UI

```md
![YARN UI](images/yarn-ui.png)
```

---

# Conclusion

This lab demonstrates:
- Manual Hadoop installation
- Hadoop service configuration
- HDFS management
- Distributed system setup
- Linux-based Big Data environments

--

#Author
Vikhom Manpiriya
