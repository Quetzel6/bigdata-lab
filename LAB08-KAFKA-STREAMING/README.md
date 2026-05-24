# LAB08 — ODBC/JDBC & Kafka Streaming

## Objective

This lab demonstrates real-time data streaming using Apache Kafka.

The project simulates a coffee shop order monitoring system where:
- Producers send order data
- Multiple consumers process the data
- Analytics and storage are performed in real time

---

# Technologies Used

- Apache Kafka
- Python
- JDBC / ODBC Concepts
- Real-time Streaming
- Distributed Messaging System

---

# System Architecture

```text
Producer (Order App)
        │
        ▼
Kafka Topic (coffee-orders)
        │
 ┌──────┼────────┐
 ▼      ▼        ▼
Dashboard Analytics Storage
Consumer Consumer Consumer
        │
        ▼
 orders.txt
```

---

# Kafka Workflow

1. Producer sends coffee order messages
2. Kafka stores messages in topic
3. Consumers subscribe to topic
4. Data is processed in real time

---

# Example Producer

```python
producer.send(
    "coffee-orders",
    {
        "order_id": 1,
        "menu": "Americano",
        "price": 60
    }
)
```

---

# Consumer Types

| Consumer | Function |
|---|---|
| Dashboard Consumer | Display incoming orders |
| Analytics Consumer | Calculate total sales |
| Storage Consumer | Save orders to file |

---

# Example Output

## Producer Output

```text
Sent: {'order_id': 1, 'menu': 'Americano', 'price': 60}
Sent: {'order_id': 2, 'menu': 'Latte', 'price': 70}
```

---

## Dashboard Consumer

```text
New Order:
{'order_id': 1, 'menu': 'Americano', 'price': 60}
```

---

## Analytics Consumer

```text
Total Sales: 60
Total Sales: 130
```

---

## Storage Consumer

```text
Saved:
{'order_id': 1, 'menu': 'Americano', 'price': 60}
```

---

# Kafka Features

- Real-time processing
- Distributed messaging
- Scalability
- Fault tolerance
- Multi-consumer architecture

---

# JDBC / ODBC Concepts

JDBC and ODBC are standard database connectivity interfaces.

| Technology | Purpose |
|---|---|
| JDBC | Java Database Connectivity |
| ODBC | Open Database Connectivity |

These technologies allow applications to connect with databases and 
streaming systems.

---

# Project Result

The system successfully:
- Streamed coffee order data
- Processed analytics in real time
- Stored data automatically
- Demonstrated Kafka architecture

---

# Screenshots

## Kafka Streaming Result

<!-- Add screenshot here -->

```md
![Kafka Result](images/kafka-dashboard.png)
```

---

# Conclusion

This lab demonstrates:
- Apache Kafka streaming
- Real-time analytics
- Producer-consumer architecture
- Distributed data processing

--

#Author
Vikhom Manpiriya
