# LAB06 — Spark MLlib with Zeppelin

## Objective

This lab demonstrates machine learning using Apache Spark MLlib through 
Zeppelin Notebook.

The lab includes:
- KMeans clustering
- Prediction model
- PySpark execution
- Zeppelin notebook analytics

---

# Technologies Used

- Apache Spark
- Spark MLlib
- Zeppelin
- PySpark
- Hadoop

---

# Architecture

```text
Dataset
   │
   ▼
PySpark
   │
   ▼
Spark MLlib
   │
   ▼
Prediction / Clustering
```

---

# Part 1 — KMeans Clustering

KMeans clustering was executed using Spark MLlib.

The model grouped data into clusters and calculated cluster centers.

---

# KMeans Result

![KMeans Result](images/kmean.png)

---

# Clustering Output

The result includes:
- Final cluster centers
- Total cost
- Spark execution logs

---

# Part 2 — Prediction Model

A prediction model was created using Zeppelin Notebook and PySpark.

The notebook predicts outcomes from input features.

---

# Prediction Notebook

![Prediction Notebook](images/test.png)

---

# Part 3 — New Predictions

Additional test data was used to generate prediction results.

---

# Prediction Results

![Prediction Results](images/พยากรณ์.png)

---

# Example PySpark Code

```python
result = model.transform(df_test)
result.select("outlook", "play", "prediction").show()
```

---

# Machine Learning Concepts

This lab demonstrates:
- Supervised learning
- Clustering
- Prediction
- Spark distributed processing

---

# Conclusion

This lab demonstrates:
- Spark MLlib analytics
- PySpark machine learning
- Zeppelin Notebook execution
- KMeans clustering
- Prediction models

--

#Author
Vikhom Manpiriya
