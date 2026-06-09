# LAB09 — PKI, Linux User Management & LDAP

## Objective

This lab introduces identity management, authentication, authorization, and 
security concepts used in Hadoop environments.

Students will learn how to:

* Create and manage Linux users
* Understand Hadoop user permissions
* Explore Public Key Infrastructure (PKI)
* Use PGP encryption and digital signatures
* Deploy and manage FreeIPA
* Configure LDAP authentication
* Integrate Hue with LDAP
* Integrate Zeppelin with LDAP

---

# Technologies Used

* Linux
* Hadoop
* HDFS
* PKI
* GnuPG
* LDAP
* FreeIPA
* Hue
* Zeppelin

---

# Part 1 — Environment Preparation

## Overview

In Hadoop environments, user authentication and authorization are commonly 
integrated with Linux accounts and centralized identity services.

This lab explores how user identities are managed across:

* Linux
* HDFS
* Hue
* Zeppelin
* LDAP

---

## Cluster Requirements

Create an EMR cluster with:

* Hive
* Hue
* Zeppelin

The cluster should be configured with a limited lifetime of approximately two 
hours.

---

## Authentication Background

If no authentication service is configured, Hadoop uses Linux users and 
groups for permission management.

Example:

```text
Linux User
      │
      ▼
 Hadoop Permission
      │
      ▼
     HDFS
```

---

# Part 2 — Linux User Management

## Step 2.1 Become Root User

```bash
sudo su -
```

Verify:

```bash
whoami
```

Expected output:

```text
root
```

---

## Step 2.2 Create a Linux User

Create a user named `nisit`.

```bash
useradd nisit
```

Verify:

```bash
id nisit
```

Expected output:

```text
uid=...
gid=...
```

---

## Step 2.3 Switch to the New User

```bash
su - nisit
```

Verify:

```bash
whoami
```

Expected output:

```text
nisit
```

---

# Part 3 — Hadoop User Directories

## Linux Home Directory

Each Linux user receives a home directory under:

```text
/home/USERNAME
```

Example:

```text
/home/nisit
```

---

## Hadoop Home Directory

HDFS maintains user directories separately.

Format:

```text
/user/USERNAME
```

Example:

```text
/user/nisit
```

---

## Step 3.1 Create HDFS Home Directory

Return to root:

```bash
sudo su -
```

Create HDFS directory:

```bash
sudo -u hdfs hadoop fs -mkdir /user/nisit
```

---

## Step 3.2 Assign Ownership

```bash
sudo -u hdfs hadoop fs -chown nisit /user/nisit
```

Verify:

```bash
hadoop fs -ls /user
```

Expected output:

```text
nisit
```

---

# Part 4 — Public Key Infrastructure (PKI)

## What is PKI

Public Key Infrastructure (PKI) is a framework used to manage digital 
identities, encryption, and trust relationships.

PKI enables:

* Encryption
* Decryption
* Authentication
* Digital Signatures
* Certificate Management

---

## PKI Components

### Public Key

Used to:

* Encrypt data
* Verify signatures

---

### Private Key

Used to:

* Decrypt data
* Create signatures

---

### Certificate Authority (CA)

A trusted organization that issues digital certificates.

---

## PKI Workflow

```text
Sender
   │
   ▼
Public Key Encryption
   │
   ▼
Encrypted Message
   │
   ▼
Private Key Decryption
   │
   ▼
Receiver
```

---

# Part 5 — PGP Fundamentals

## What is PGP

Pretty Good Privacy (PGP) is a cryptographic system used for:

* Encryption
* Decryption
* Digital Signatures

---

## PGP Public Key Block

Example:

```text
-----BEGIN PGP PUBLIC KEY BLOCK-----
...
-----END PGP PUBLIC KEY BLOCK-----
```

A public key can be shared safely.

---

## PGP Encrypted Message

Example:

```text
-----BEGIN PGP MESSAGE-----
...
-----END PGP MESSAGE-----
```

Only authorized recipients can decrypt the message.

---

## PGP Signed Message

Example:

```text
-----BEGIN PGP SIGNED MESSAGE-----
...
-----BEGIN PGP SIGNATURE-----
...
```

Digital signatures verify authenticity and integrity.

---

# Part 6 — GnuPG Installation

## Overview

GnuPG (GPG) is an open-source implementation of the OpenPGP standard.

Website:

```text
https://gnupg.org
```

---

## Install GnuPG

Download the latest version from the official website.

Verify installation:

```bash
gpg --version
```

Expected output:

```text
gpg (GnuPG)
```

---

## Common Commands

Generate key pair:

```bash
gpg --full-generate-key
```

List public keys:

```bash
gpg --list-keys
```

List secret keys:

```bash
gpg --list-secret-keys
```

These commands will be used in the following sections.

---

# Part 7 — Key Generation, Encryption and Signing

## Overview

GnuPG allows users to generate cryptographic key pairs, encrypt messages, 
decrypt messages, and verify digital signatures.

The workflow is:

```text
Generate Key Pair
        │
        ▼
Export Public Key
        │
        ▼
Encrypt Message
        │
        ▼
Decrypt Message
        │
        ▼
Sign Message
        │
        ▼
Verify Signature
```

---

## Step 7.1 Generate Key Pair

Generate a new OpenPGP key pair.

```bash
gpg --full-generate-key
```

Recommended options:

```text
Key Type: RSA and RSA
Key Size: 4096
Expiration: 0 (Never Expire)
```

Provide:

```text
Real Name
Email Address
Passphrase
```

---

## Step 7.2 Verify Generated Keys

List available public keys:

```bash
gpg --list-keys
```

Expected output:

```text
pub rsa4096
uid User Name <user@example.com>
```

List secret keys:

```bash
gpg --list-secret-keys
```

Expected output:

```text
sec rsa4096
```

---

## Step 7.3 Export Public Key

Export the public key:

```bash
gpg --armor --export user@example.com > publickey.asc
```

Verify:

```bash
cat publickey.asc
```

Expected output:

```text
-----BEGIN PGP PUBLIC KEY BLOCK-----
...
-----END PGP PUBLIC KEY BLOCK-----
```

---

## Step 7.4 Encrypt a Message

Create a text file:

```bash
echo "Hello Hadoop Security" > message.txt
```

Encrypt the file:

```bash
gpg --encrypt \
    --recipient user@example.com \
    message.txt
```

Verify:

```bash
ls
```

Expected output:

```text
message.txt.gpg
```

---

## Step 7.5 Decrypt a Message

Decrypt the encrypted file:

```bash
gpg --decrypt message.txt.gpg
```

Expected output:

```text
Hello Hadoop Security
```

---

## Step 7.6 Create a Digital Signature

Create a signed file:

```bash
gpg --clearsign message.txt
```

Verify:

```bash
cat message.txt.asc
```

Expected output:

```text
-----BEGIN PGP SIGNED MESSAGE-----
...
-----BEGIN PGP SIGNATURE-----
...
```

---

## Step 7.7 Verify Signature

Verify the signature:

```bash
gpg --verify message.txt.asc
```

Expected output:

```text
Good signature
```

---

# Part 8 — FreeIPA Overview

## What is FreeIPA

FreeIPA is an open-source identity management platform.

It combines several technologies:

* LDAP
* Kerberos
* DNS
* Certificate Management
* Policy Management

---

## Why FreeIPA

Large Hadoop environments may contain:

* Hundreds of users
* Multiple Hadoop nodes
* Multiple web applications

Managing accounts individually becomes difficult.

FreeIPA centralizes:

```text
Authentication
Authorization
Identity Management
```

---

## FreeIPA Architecture

```text
Users
   │
   ▼
FreeIPA
   │
   ├── LDAP
   ├── Kerberos
   ├── DNS
   └── Certificates
   │
   ▼
Hadoop Services
```

---

## Deployment Overview

In this lab:

```text
FreeIPA Server
      │
      ▼
LDAP Authentication
      │
      ▼
Hue
Zeppelin
Linux Login
```

---

# Part 9 — FreeIPA Server Installation

## Step 9.1 Create EC2 Instance

Create a Red Hat instance:

```text
Instance Type: t2.medium
vCPU: 2
Memory: 4 GB
Operating System: Red Hat Enterprise Linux 10
```

---

## Step 9.2 Connect via SSH

```bash
ssh -i key.pem ec2-user@PUBLIC_IP
```

Switch to root:

```bash
sudo su -
```

---

## Step 9.3 Verify Hostname

Display current hostname:

```bash
hostname
```

Record the hostname before installation.

---

## Step 9.4 Update Packages

```bash
yum update -y
```

---

## Step 9.5 Install FreeIPA Server

```bash
yum install -y ipa-server
```

Verify:

```bash
rpm -qa | grep ipa-server
```

---

## Step 9.6 Start Installation

```bash
ipa-server-install
```

Use default values unless otherwise specified.

Important values:

```text
Server Host Name:
ldap.example.com

Domain:
example.com

Realm:
EXAMPLE.COM
```

---

## Step 9.7 Configure Firewall

Allow LDAP traffic:

```bash
firewall-cmd \
    --add-service=freeipa-ldap \
    --permanent
```

Apply configuration:

```bash
firewall-cmd \
    --add-service=freeipa-ldap
```

---

## Step 9.8 Verify Services

Check FreeIPA status:

```bash
ipactl status
```

Expected services:

```text
Directory Service
Kerberos
HTTP
```

---

## Step 9.9 Obtain Kerberos Ticket

```bash
kinit admin
```

Verify:

```bash
klist
```

Expected output:

```text
Ticket cache
Principal: admin@EXAMPLE.COM
```

---

## Step 9.10 Access FreeIPA Web Console

Update local hosts file:

Linux:

```text
/etc/hosts
```

Windows:

```text
C:\Windows\System32\drivers\etc\hosts
```

Add:

```text
13.218.62.3 ldap.example.com
```

Open browser:

```text
https://ldap.example.com
```

Login:

```text
Username: admin
```

---

# Part 10 — FreeIPA Client Installation

## Overview

The Hadoop primary node will be configured as a FreeIPA client.

In production environments all nodes should join the domain.

In this lab only the primary node is required.

---

## Step 10.1 Install Client Package

```bash
sudo su -

yum install -y ipa-client
```

---

## Step 10.2 Configure Hostname

```bash
hostnamectl \
set-hostname \
primary.example.com
```

Verify:

```bash
hostname
```

Expected output:

```text
primary.example.com
```

---

## Step 10.3 Configure Hosts File

Edit:

```bash
vi /etc/hosts
```

Add:

```text
13.218.62.3 ldap.example.com
```

This allows hostname resolution without DNS.

---

## Step 10.4 Join FreeIPA Domain

```bash
ipa-client-install \
    --server=ldap.example.com \
    --domain=EXAMPLE.COM \
    --mkhomedir
```

When prompted:

```text
Proceed with fixed values and no DNS discovery?
yes
```

Authenticate using:

```text
admin
```

and the IPA administrator password.

---

## Step 10.5 Verify Enrollment

Verify Kerberos authentication:

```bash
kinit admin
```

Check ticket:

```bash
klist
```

The system should display a valid Kerberos ticket.

---

# Part 11 — LDAP Commands

## Overview

LDAP (Lightweight Directory Access Protocol) is used to query and manage 
directory information.

In this lab, LDAP is provided through FreeIPA.

---

## Step 11.1 Install LDAP Client Tools

```bash
sudo su -

yum install -y openldap-clients
```

Verify:

```bash
ldapsearch -VV
```

---

## Step 11.2 Query LDAP Directory

Execute:

```bash
ldapsearch -x -b "dc=example,dc=com"
```

Expected output:

```text
dn: dc=example,dc=com
objectClass: top
objectClass: domain
```

This command retrieves directory information from LDAP.

---

## Step 11.3 Obtain Kerberos Ticket

```bash
kinit admin
```

Verify:

```bash
klist
```

Expected output:

```text
Principal: admin@EXAMPLE.COM
```

---

# Part 12 — FreeIPA User Management

## Overview

FreeIPA provides centralized user management for Linux and Hadoop 
environments.

---

## Step 12.1 Login to FreeIPA Web Console

Open:

```text
https://ldap.example.com
```

Login using:

```text
Username: admin
Password: [IPA Administrator Password]
```

---

## Step 12.2 Create a New User

Navigate to:

```text
Identity
 └── Users
      └── Add
```

Provide:

```text
Username
First Name
Last Name
Password
```

Save the user.

---

## Step 12.3 Verify User Creation

On the Hadoop node:

```bash
id username
```

or

```bash
getent passwd username
```

The LDAP user should appear in the system.

---

# Part 13 — Hue LDAP Authentication

## Overview

Hue can authenticate users directly against the FreeIPA LDAP directory.

This removes the need to manage local Hue accounts.

---

## Step 13.1 Configure Hue Authentication

Edit:

```bash
vi /etc/hue/conf/hue.ini
```

Locate and configure:

```ini
backend=desktop.auth.backend.LdapBackend
```

---

## Step 13.2 Configure LDAP Settings

```ini
[[[[mycompany]]]]

base_dn="dc=example,dc=com"

ldap_url=ldaps://ldap.example.com

ldap_username_pattern="uid=<username>,cn=users,cn=accounts,dc=example,dc=com"

search_bind_authentication=true

create_users_on_login=true
```

---

## Step 13.3 Configure User Filter

```ini
[[[[[users]]]]]

user_filter="objectclass=person"

user_name_attr=uid
```

---

## Step 13.4 Configure Group Filter

```ini
[[[[[groups]]]]]

group_filter="objectclass=posixgroup"

group_name_attr=cn
```

---

## Step 13.5 Restart Hue

```bash
systemctl restart hue
```

Verify:

```bash
systemctl status hue
```

---

## Step 13.6 Test LDAP Login

Open Hue.

Login using:

```text
LDAP Username
LDAP Password
```

The first successful login becomes the Hue administrator.

---

# Part 14 — Zeppelin LDAP Authentication

## Overview

Zeppelin can authenticate users through the same LDAP directory.

---

## Step 14.1 Copy Configuration Template

```bash
cd /etc/zeppelin/conf

cp shiro.ini.template shiro.ini
```

---

## Step 14.2 Configure LDAP Realm

Edit:

```bash
vi shiro.ini
```

Configure:

```ini
ldapRealm = org.apache.zeppelin.realm.LdapGroupRealm

ldapRealm.contextFactory.environment[ldap.searchBase] = dc=example,dc=com

ldapRealm.contextFactory.url = ldaps://ldap.example.com

ldapRealm.userDnTemplate = uid={0},cn=users,cn=accounts,dc=example,dc=com

ldapRealm.contextFactory.authenticationMechanism = SIMPLE
```

---

## Step 14.3 Restart Zeppelin

```bash
systemctl restart zeppelin
```

Verify:

```bash
systemctl status zeppelin
```

---

## Step 14.4 Test LDAP Login

Open Zeppelin.

Login using the LDAP account created in FreeIPA.

Successful login confirms LDAP integration.

---

# Part 15 — Zeppelin User Impersonation

## Overview

User impersonation allows Zeppelin notebooks to execute as the currently 
logged-in Hadoop user.

This ensures correct HDFS permissions.

---

## Step 15.1 Configure Hadoop Proxy User

Edit:

```bash
vi /etc/hadoop/conf/core-site.xml
```

Add:

```xml
<property>
  <name>hadoop.proxyuser.zeppelin.groups</name>
  <value>*</value>
</property>

<property>
  <name>hadoop.proxyuser.zeppelin.users</name>
  <value>*</value>
</property>

<property>
  <name>hadoop.proxyuser.zeppelin.hosts</name>
  <value>*</value>
</property>
```

---

## Step 15.2 Configure Zeppelin Interpreter

Open Zeppelin.

Navigate:

```text
Interpreter
  └── Spark
```

Edit configuration:

```text
The interpreter will be instantiated Per User in isolated process
```

Enable:

```text
User Impersonate
```

Save changes.

---

## Step 15.3 Verify Impersonation

Execute:

```python
import os

print(os.getenv("USER"))
```

The output should match the currently logged-in LDAP user.

---

# Part 16 — Screenshots

The following screenshots were captured during the execution of this lab.

---

## SSH Key Generation

The screenshot below shows SSH key generation using OpenSSH.

![SSH Key Generation](images/ssh-keygen.png)

---

## Generated SSH Keys

The generated key pair consists of:

* id_rsa
* id_rsa.pub

![Generated SSH Keys](images/ssh-keys.png)

---

## SSH Authentication

The following screenshot demonstrates successful SSH login using public key 
authentication.

![SSH Login](images/ssh-login.png)

---

# Part 17 — Troubleshooting

## FreeIPA Installation Failed

Verify package installation:

```bash
rpm -qa | grep ipa
```

---

## Kerberos Ticket Not Found

Acquire a new ticket:

```bash
kinit admin
```

Verify:

```bash
klist
```

---

## LDAP Search Failed

Verify hostname resolution:

```bash
ping ldap.example.com
```

Check:

```bash
cat /etc/hosts
```

---

## Hue LDAP Login Failed

Verify:

```bash
systemctl status hue
```

Check LDAP configuration in:

```text
/etc/hue/conf/hue.ini
```

---

## Zeppelin LDAP Login Failed

Verify:

```bash
systemctl status zeppelin
```

Review:

```text
/etc/zeppelin/conf/shiro.ini
```

---

## SSH Permission Denied (publickey)

Verify permissions:

```bash
chmod 700 ~/.ssh

chmod 600 ~/.ssh/id_rsa
```

---

# Part 18 — Conclusion

In this lab, we learned how to:

* Create and manage Linux users
* Create Hadoop user directories
* Understand Public Key Infrastructure (PKI)
* Generate and manage PGP keys
* Encrypt and sign messages using GnuPG
* Deploy and manage FreeIPA
* Query LDAP directories
* Authenticate Hue using LDAP
* Authenticate Zeppelin using LDAP
* Configure Zeppelin user impersonation

These technologies provide the foundation for centralized identity 
management, authentication, authorization, and secure access within modern 
Hadoop environments.

---

# Author

Vikhom Manpiriya

Student ID: 66102010185

