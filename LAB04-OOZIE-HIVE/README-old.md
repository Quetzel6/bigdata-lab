# LAB04 — Oozie Workflow & Hive Query

## Objective

This lab demonstrates:

- Creating Oozie workflows
- Running Sqoop and Hive jobs
- Querying Hive tables using Hue
- Verifying Hive analytics results

---

# Technologies Used

- AWS EMR
- Oozie
- Hive
- Hue
- Hadoop

---

# Architecture Overview

```text
MySQL
   │
   ▼
 Sqoop
   │
   ▼
 Hive
   │
   ▼
 Hue Query Editor
```

---

# Step 1 — Create Oozie Workflow

The workflow imports data from MySQL using Sqoop and processes it in Hive.

Workflow components:
- Sqoop Import
- Hive Query

---

# Oozie Workflow

![Oozie Workflow](images/oozie-workflow.png)

---

# Step 2 — Execute Hive Query

Example query:

```sql
SELECT * FROM day_count LIMIT 10;
```

---

# Hive Query Result

The following image shows successful Hive query execution in Hue.

![Hive Query](images/hive-daycount-query.png)

---

# Table Information

The `day_count` table contains:

- count_date (timestamp)
- mem_count (integer)

---

# Conclusion

This lab demonstrates:
- Oozie orchestration
- Sqoop integration
- Hive querying
- Hue analytics workflow

--

#Author
Vikhom Manpiriya
