# LAB 01 — AWS EC2 & EMR Environment Setup

## Objective
This lab demonstrates how to create an AWS EC2 instance and prepare the 
environment for Big Data laboratories using Amazon Linux and SSH access.

---

# Prerequisites

- AWS Academy Account
- AWS Academy Learner Lab Access
- SSH Client
  - macOS/Linux Terminal
  - Windows PowerShell or Git Bash

---

# Step 1 — Launch AWS Academy Learner Lab

1. Open AWS Academy

https://awsacademy.instructure.com/

2. Navigate to:

AWS Academy Learner Lab → Launch AWS Academy Learner Lab

3. Click:

Start Lab

4. Wait until AWS status becomes green.

5. Click AWS to open AWS Console.

---

# Step 2 — Create EC2 Instance

## 2.1 Open EC2 Dashboard

Search for:

EC2

Then select:

Instances → Launch an instance

---

## 2.2 Configure Instance

### Name and Tags

Example:

```text
My Linux Server
```

---

### Amazon Machine Image (AMI)

Select:

```text
Amazon Linux
```

> Amazon Linux is based on Red Hat Enterprise Linux (RHEL).

---

### Instance Type

Select:

```text
t3.micro
```

---

### Key Pair (Login)

- Use existing key pair
OR
- Create a new key pair

Download the `.pem` file and keep it secure.

---

# Step 3 — Configure Security Group

## Inbound Rules

Add rule:

| Type | Source |
|---|---|
| All Traffic | My IP |

> This allows your machine to connect to the EC2 instance.

---

# Step 4 — Launch Instance

Click:

```text
Launch Instance
```

Wait until the instance status becomes:

```text
Running
```

---

# Step 5 — Connect via SSH

## macOS / Linux

```bash
ssh -i [private-key.pem] ec2-user@[PUBLIC-IP]
```

Example:

```bash
ssh -i mykey.pem ec2-user@54.123.45.67
```

---

# Windows Permission Denied Fix

If Windows shows:

```text
Permission denied (publickey)
```

Run:

```powershell
icacls "C:\path\to\key.pem" /inheritance:r /grant:r "$($env:USERNAME):(R)"
```

---

# Common Errors

## Error: Permission denied (publickey)

### Causes
- Wrong `.pem` file
- Incorrect file permissions
- Wrong username
- Security Group does not allow SSH

### Solutions
- Verify `.pem` file
- Use correct username:
  - Amazon Linux → `ec2-user`
- Ensure port 22 is open in Security Group

---

# Expected Result

Successfully connected to EC2 instance via SSH.

Example:

```text
[ec2-user@ip-172-31-xx-xx ~]$
```

---

# Screenshots to Capture

- AWS Learner Lab Status
- EC2 Instance Running State
- Security Group Inbound Rules
- SSH Terminal Connection

---

# Author

Vikhom Manpiriya
