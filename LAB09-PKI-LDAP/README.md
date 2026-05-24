# LAB09 — PKI, User Management & LDAP

## Objective

This lab demonstrates:

- Linux user management
- SSH authentication
- Public Key Infrastructure (PKI)
- LDAP concepts and centralized authentication

---

# Technologies Used

- Linux
- SSH
- OpenSSH
- PKI
- LDAP

---

# Architecture Overview

```text
User
  │
  ▼
SSH Authentication
  │
  ▼
Public / Private Key
  │
  ▼
Linux Server
```

---

# Part 1 — Linux User Management

## Create User

```bash
sudo useradd student1
```

---

## Set Password

```bash
sudo passwd student1
```

---

## Verify User

```bash
cat /etc/passwd
```

---

# Part 2 — Public Key Infrastructure (PKI)

## Generate SSH Key

```bash
ssh-keygen -t rsa
```

---

## SSH Key Files

| File | Description |
|---|---|
| id_rsa | Private Key |
| id_rsa.pub | Public Key |

---

## Copy Public Key

```bash
ssh-copy-id user@server-ip
```

---

## SSH Login

```bash
ssh user@server-ip
```

---

# Part 3 — LDAP Concepts

LDAP (Lightweight Directory Access Protocol) is used for centralized 
authentication and user management.

LDAP allows:
- Centralized user accounts
- Authentication services
- Access management
- Directory services

---

# LDAP Architecture

```text
Client
   │
   ▼
LDAP Server
   │
   ▼
User Directory Database
```

---

# Security Concepts

## Public Key Authentication

Public key authentication is more secure than password authentication 
because:
- No password transmission
- Strong encryption
- Identity verification

---

# Example Linux Commands

## Current User

```bash
whoami
```

---

## List Groups

```bash
groups
```

---

## Switch User

```bash
su - student1
```

---

# Screenshots

## User Management

```md
![User Management](images/user-management.png)
```

---

## SSH Key Generation

```md
![SSH Key](images/ssh-keygen.png)
```

---

## SSH Login

```md
![SSH Login](images/ssh-login.png)
```

---

# Conclusion

This lab demonstrates:
- Linux user administration
- SSH authentication
- PKI fundamentals
- LDAP concepts
- Secure remote access

--

#Author
Vikhom Manpiriya
