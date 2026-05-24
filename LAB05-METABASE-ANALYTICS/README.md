# LAB05 — Metabase Dashboard Analytics

## Objective

This lab demonstrates Business Intelligence and analytics visualization 
using:

- Presto
- Hive
- Metabase
- Hadoop ecosystem

The dashboard visualizes sales and business metrics from the Northwind 
dataset.

---

# Technologies Used

- AWS EMR
- Hive
- Presto
- Metabase
- Hadoop

---

# Dashboard Overview

The Metabase dashboard was connected directly to Presto and visualized 
business data stored in Hive tables.

---

# Dashboard Screenshot

![Metabase Dashboard](images/dashboard.png)

---

# Dashboard Components

## 1. Top 10 Best Seller

Displays products with the highest sales quantity.

Chart Type:
- Bar Chart

---

## 2. Low Performance Products

Displays products with lower sales quantity.

Chart Type:
- Bar Chart

---

## 3. Sales by Month

Displays monthly sales trends over time.

Chart Type:
- Line Chart

---

## 4. Sales by Country

Displays percentage contribution by country.

Chart Type:
- Pie / Donut Chart

---

# Example Analytics Queries

## Monthly Sales

```sql
SELECT
  month(orderdate) AS month,
  SUM(quantity * unitprice) AS sales
FROM orders
GROUP BY month(orderdate)
ORDER BY month;
```

---

# Benefits of BI Dashboard

- Easy data visualization
- Business trend analysis
- Sales monitoring
- Product performance comparison

---

# Conclusion

This lab demonstrates:
- Connecting Metabase with Presto
- Building analytics dashboards
- Visualizing business metrics
- Using Hive data for BI reporting

--

#Author
Vikhom Manpiriya
