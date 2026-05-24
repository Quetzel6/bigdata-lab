# LAB01 — AWS EMR Hadoop Cluster Setup

## Objective

This lab demonstrates how to create and configure an AWS EC2 instance for 
Big Data Engineering environments.

Students learn:
- How to launch AWS EC2 instances
- Basic Linux server access
- SSH connection setup
- Security Group configuration
- Preparation for Hadoop ecosystem tools

---

# Environment

| Component | Description |
|---|---|
| Cloud Platform | AWS Academy Learner Lab |
| Operating System | Amazon Linux |
| Instance Type | t3.micro |
| Access Method | SSH |
| Key Authentication | PEM Key Pair |

---

# AWS Academy Setup

1. Open AWS Academy Learner Lab
2. Start Lab
3. Wait until AWS status becomes green
4. Open AWS Console

---

# EC2 Instance Configuration

## Step 1 — Open EC2 Dashboard

Navigate to:

```text
AWS Console → EC2 → Instances
```

---

## Step 2 — Launch Instance

Click:

```text
Launch Instance
```

---

## Step 3 — Configure Instance

### Name and Tags

Example:

```text
My Linux Server
```

---

### Quick Start

Select:

```text
Amazon Linux
```

Amazon Linux is based on Red Hat Enterprise Linux (RHEL).

---

### Instance Type

Select:

```text
t3.micro
```

---

### Key Pair

Choose:
- Existing key pair
OR
- Create new key pair

Example:

```text
my-key.pem
```

---

# Security Group Configuration

## Inbound Rule

Add rule:

| Type | Source |
|---|---|
| All Traffic | My IP |

This allows remote access to the instance.

---

# Launch Instance

Click:

```text
Launch Instance
```

Wait until instance status becomes:

```text
Running
```

---

# Connect to EC2 via SSH

## SSH Command

```bash
ssh -i [private-key.pem] ec2-user@[PUBLIC-IP]
```

Example:

```bash
ssh -i my-key.pem ec2-user@18.123.45.67
```

---

# Resolve Permission Denied Error (Windows)

If SSH key permission error occurs:

```bash
icacls "C:\path\to\key.pem" /inheritance:r /grant:r "$($env:USERNAME):(R)"
```

---

# Basic Linux Commands

## Check Current Directory

```bash
pwd
```

---

## List Files

```bash
ls
```

---

## Change Directory

```bash
cd
```

---

## Create Folder

```bash
mkdir test
```

---

## Create File

```bash
touch hello.txt
```

---

## Edit File

```bash
nano hello.txt
```

---

## Remove File

```bash
rm hello.txt
```

---

# Learning Outcomes

After completing this lab, students can:
- Launch AWS EC2 instances
- Configure cloud networking
- Connect Linux servers remotely
- Use basic Linux commands
- Prepare environments for Hadoop ecosystem tools

---

# Screenshots

## AWS EC2 Instance

```md
![EC2 Instance](images/ec2-instance.png)
```

---

## SSH Connection

```md
![SSH Connection](images/ssh-connect.png)
```

---

## Linux Command Example

```md
![Linux Commands](images/linux-command.png)
```

---

# Conclusion

This lab provides foundational skills for Big Data Engineering 
environments using AWS cloud infrastructure and Linux systems.  
The configured EC2 instance will be used in later labs involving Hadoop, 
Hive, Spark, Kafka, and other Big Data tools.

--

#Author
Vikhom Manpiriya
