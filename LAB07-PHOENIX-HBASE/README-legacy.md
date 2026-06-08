# LAB07 — Apache Phoenix with HBase

## Objective

This lab demonstrates how to use Apache Phoenix as a SQL layer on top of 
HBase.

The lab includes:
- Connecting to Phoenix
- Viewing tables
- Inserting data
- Querying data using SQL

---

# Technologies Used

- Apache HBase
- Apache Phoenix
- SQLLine
- Hadoop Ecosystem

---

# Architecture

```text
Application
    │
    ▼
Phoenix SQL
    │
    ▼
HBase
    │
    ▼
HDFS
```

---

# Step 1 — Connect to Phoenix

Phoenix SQLLine was used to connect to the Phoenix server.

Example:

```bash
sqlline.py localhost
```

---

# Step 2 — Show Tables

The following command displays available tables in Phoenix.

```sql
!tables
```

---

# Step 3 — Insert Data

Data was inserted using UPSERT.

Example:

```sql
UPSERT INTO MEMBER VALUES
(1,'Vikhom','Manpiriya','2026-03-30 16:00:00');
```

---

# Step 4 — Query Data

Data was retrieved using SQL queries.

Example:

```sql
SELECT * FROM MEMBER;
```

---

# Phoenix Query Result

![Phoenix Query](images/phoenix-query.png)

---

# Result Description

The result shows:
- MEMBER table
- Inserted records
- Query execution
- SQL output from Phoenix

---

# SQL Operations Used

| Operation | Description |
|---|---|
| !tables | Show all tables |
| UPSERT | Insert or update data |
| SELECT | Retrieve data |

---

# Benefits of Phoenix

- SQL support on HBase
- Fast querying
- Distributed storage
- Scalable NoSQL integration

---

# Conclusion

This lab demonstrates:
- Apache Phoenix integration
- SQL operations on HBase
- Data insertion and querying
- Hadoop ecosystem storage

--

#Author
Vikhom Manpiriya
