# 🍕 Pizza Sales Analysis — SQL Project

A MySQL-based data analysis project for analyzing pizza sales, customer orders, revenue, product performance, and sales trends using SQL queries.

## 📌 Overview

The **Pizza Sales Analysis** project uses SQL to analyze transactional pizza sales data and extract meaningful business insights.

The analysis covers:

* Total number of orders
* Total revenue generated
* Highest-priced pizza
* Most commonly ordered pizza size
* Top 5 most ordered pizza types
* Pizza category performance
* Hourly order distribution
* Average pizzas ordered per day
* Top pizzas by revenue
* Revenue contribution by category
* Cumulative revenue over time
* Top 3 revenue-generating pizzas within each category

## 🎯 Project Objectives

The main objectives are to:

1. Analyze overall pizza sales performance.
2. Identify the best-selling pizza products.
3. Determine which pizza categories generate the most sales.
4. Analyze customer ordering patterns by time and date.
5. Calculate revenue and revenue contribution.
6. Use advanced SQL techniques such as subqueries and window functions.
7. Generate business insights that can support sales and marketing decisions.

## 🛠️ Technologies Used

* **Database:** MySQL
* **Language:** SQL
* **Concepts:** Joins, Aggregations, Subqueries, Window Functions
* **Functions:** `COUNT()`, `SUM()`, `AVG()`, `ROUND()`, `HOUR()`
* **Clauses:** `GROUP BY`, `ORDER BY`, `WHERE`, `LIMIT`
* **Advanced SQL:** `RANK()` and cumulative `SUM() OVER()`

## 🗄️ Database Schema

The project works with four main tables:

### 1. `orders`

Stores information about customer orders.

| Column       | Data Type | Description             |
| ------------ | --------- | ----------------------- |
| `order_id`   | INT       | Unique order identifier |
| `order_date` | DATE      | Date of the order       |
| `order_time` | TIME      | Time of the order       |

**Primary Key:** `order_id`

### 2. `order_details`

Stores individual pizza items belonging to each order.

| Column             | Data Type | Description                    |
| ------------------ | --------- | ------------------------------ |
| `order_details_id` | INT       | Unique order-detail identifier |
| `order_id`         | INT       | Related order                  |
| `pizza_id`         | TEXT      | Related pizza                  |
| `quantity`         | INT       | Quantity ordered               |

**Primary Key:** `order_details_id`

### 3. `pizzas`

Stores pizza-level information.

| Column          | Description             |
| --------------- | ----------------------- |
| `pizza_id`      | Unique pizza identifier |
| `pizza_type_id` | Related pizza type      |
| `size`          | Pizza size              |
| `price`         | Pizza price             |

### 4. `pizza_types`

Stores information about pizza types.

| Column          | Description                  |
| --------------- | ---------------------------- |
| `pizza_type_id` | Unique pizza type identifier |
| `name`          | Pizza name                   |
| `category`      | Pizza category               |

## 🔗 Table Relationships

```text
pizza_types
    │
    │ pizza_type_id
    ▼
pizzas
    │
    │ pizza_id
    ▼
order_details
    │
    │ order_id
    ▼
orders
```

The relationships allow sales transactions to be connected with pizza names, sizes, categories, prices, dates, and order times.

---

# ⚙️ Setup

## 1. Create the Database

```sql
CREATE DATABASE pizzahut;

USE pizzahut;
```

## 2. Create the Orders Table

```sql
CREATE TABLE orders (
    order_id INT NOT NULL,
    order_date DATE NOT NULL,
    order_time TIME NOT NULL,
    PRIMARY KEY (order_id)
);
```

## 3. Create the Order Details Table

```sql
CREATE TABLE order_details (
    order_details_id INT NOT NULL,
    order_id INT NOT NULL,
    pizza_id TEXT NOT NULL,
    quantity INT NOT NULL,
    PRIMARY KEY (order_details_id)
);
```

> **Note:** The uploaded SQL file references the `pizzas` and `pizza_types` tables. Those tables must also exist and contain the corresponding pizza and pricing data before running the analysis queries.

---

# 📊 Key SQL Analysis

## 1. Calculate Total Orders

This query counts the total number of orders placed.

```sql
SELECT
    COUNT(order_id) AS total_orders
FROM orders;
```

### Business Insight

This provides the overall number of transactions and gives a basic measurement of sales volume.

---

## 2. Calculate Total Revenue

This query calculates total revenue by multiplying the quantity sold by the pizza price.

```sql
SELECT
    ROUND(SUM(order_details.quantity * pizzas.price), 2) AS total_sales
FROM order_details
JOIN pizzas
    ON pizzas.pizza_id = order_details.pizza_id;
```

### Business Insight

The result represents the total sales revenue generated from pizza orders.

---

## 3. Find the Top 3 Pizzas by Revenue

This query identifies the three pizza types that generate the highest revenue.

```sql
SELECT
    pizza_types.name,
    SUM(order_details.quantity * pizzas.price) AS revenue
FROM pizza_types
JOIN pizzas
    ON pizzas.pizza_type_id = pizza_types.pizza_type_id
JOIN order_details
    ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY revenue DESC
LIMIT 3;
```

### How It Works

1. Joins pizza types with pizza details.
2. Connects pizzas with order transactions.
3. Calculates revenue using `quantity × price`.
4. Groups revenue by pizza name.
5. Sorts pizzas from highest to lowest revenue.
6. Returns the top three.

---

# 📈 Advanced SQL Analysis

## Cumulative Revenue Over Time

Window functions can be used to calculate cumulative revenue.

```sql
SELECT
    order_date,
    SUM(revenue) OVER (
        ORDER BY order_date
    ) AS cumulative_revenue
FROM (
    SELECT
        orders.order_date,
        SUM(order_details.quantity * pizzas.price) AS revenue
    FROM order_details
    JOIN pizzas
        ON order_details.pizza_id = pizzas.pizza_id
    JOIN orders
        ON orders.order_id = order_details.order_id
    GROUP BY orders.order_date
) AS sales;
```

This helps identify how revenue accumulates over the sales period.

---

# 🏆 Top 3 Pizzas by Revenue in Each Category

The project also uses `RANK()` to find the highest-revenue pizzas within every category.

```sql
SELECT
    name,
    category,
    revenue
FROM (
    SELECT
        pizza_types.category,
        pizza_types.name,
        SUM(order_details.quantity * pizzas.price) AS revenue,
        RANK() OVER (
            PARTITION BY pizza_types.category
            ORDER BY SUM(order_details.quantity * pizzas.price) DESC
        ) AS ranking
    FROM pizza_types
    JOIN pizzas
        ON pizza_types.pizza_type_id = pizzas.pizza_type_id
    JOIN order_details
        ON order_details.pizza_id = pizzas.pizza_id
    GROUP BY
        pizza_types.category,
        pizza_types.name
) AS ranked_pizzas
WHERE ranking <= 3;
```

### Why `RANK()`?

`RANK()` ranks pizzas separately within each category. Using:

```sql
PARTITION BY category
```

resets the ranking for every pizza category.

---

# ✏️ Data Manipulation Examples

## Insert a New Order

```sql
INSERT INTO orders (
    order_id,
    order_date,
    order_time
)
VALUES (
    10001,
    '2026-08-08',
    '18:30:00'
);
```

## Update Order Information

```sql
UPDATE orders
SET order_time = '19:00:00'
WHERE order_id = 10001;
```

## Delete an Order

```sql
DELETE FROM orders
WHERE order_id = 10001;
```

> In a production database, related `order_details` records should be handled carefully before deleting an order.

---

# 🔍 Additional Analysis Queries

### Highest-Priced Pizza

```sql
SELECT
    pizza_types.name,
    pizzas.price
FROM pizza_types
JOIN pizzas
    ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;
```

### Most Common Pizza Size

```sql
SELECT
    pizzas.size,
    COUNT(order_details.order_details_id) AS order_count
FROM pizzas
JOIN order_details
    ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY order_count DESC;
```

### Top 5 Most Ordered Pizza Types

```sql
SELECT
    pizza_types.name,
    SUM(order_details.quantity) AS quantity
FROM pizza_types
JOIN pizzas
    ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details
    ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name
ORDER BY quantity DESC
LIMIT 5;
```

### Orders by Hour

```sql
SELECT
    HOUR(order_time) AS hour,
    COUNT(order_id) AS order_count
FROM orders
GROUP BY HOUR(order_time)
ORDER BY hour;
```

### Pizza Quantity by Category

```sql
SELECT
    pizza_types.category,
    SUM(order_details.quantity) AS quantity
FROM pizza_types
JOIN pizzas
    ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details
    ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category
ORDER BY quantity DESC;
```

### Average Pizzas Ordered Per Day

```sql
SELECT
    ROUND(AVG(quantity), 0) AS avg_pizza_ordered_per_day
FROM (
    SELECT
        orders.order_date,
        SUM(order_details.quantity) AS quantity
    FROM orders
    JOIN order_details
        ON orders.order_id = order_details.order_id
    GROUP BY orders.order_date
) AS order_quantity;
```

---

# 🧠 SQL Concepts Demonstrated

This project demonstrates practical knowledge of:

* `CREATE DATABASE`
* `CREATE TABLE`
* `INSERT`
* `UPDATE`
* `DELETE`
* `SELECT`
* `INNER JOIN`
* `GROUP BY`
* `ORDER BY`
* `LIMIT`
* Aggregate functions
* Subqueries
* Date and time functions
* Revenue calculations
* Window functions
* `SUM() OVER()`
* `RANK()`
* `PARTITION BY`

---

# 📌 Business Questions Answered

The project answers questions such as:

1. How many orders were placed?
2. How much total revenue was generated?
3. Which pizza has the highest price?
4. Which pizza size is ordered most frequently?
5. What are the top 5 most ordered pizzas?
6. Which pizza categories sell the most?
7. During which hours are the most orders placed?
8. How many pizzas are ordered on average per day?
9. Which pizzas generate the most revenue?
10. What percentage of revenue comes from each category?
11. How does cumulative revenue change over time?
12. Which are the top 3 pizzas in each category?

---

# 📂 Project Structure

```text
Pizza-Sales-Analysis/
│
├── README.md
│
├── Pizza Sales Analysis.sql
│
└── dataset/
    ├── orders.csv
    ├── order_details.csv
    ├── pizzas.csv
    └── pizza_types.csv
```

---

# 🚀 How to Run

1. Install **MySQL Server** and a MySQL client such as MySQL Workbench.
2. Open the SQL project file.
3. Execute:

```sql
CREATE DATABASE pizzahut;
USE pizzahut;
```

4. Create/import the required tables and data.
5. Execute the analysis queries.
6. Review the returned results to identify sales trends and top-performing pizzas.

---

# 🧪 Testing Checklist

* [ ] Database created successfully.
* [ ] `orders` table created.
* [ ] `order_details` table created.
* [ ] `pizzas` table available.
* [ ] `pizza_types` table available.
* [ ] Data imported successfully.
* [ ] Joins return expected results.
* [ ] Revenue calculations execute correctly.
* [ ] Window-function queries execute correctly.
* [ ] Ranking queries return the expected top pizzas.

---

# ⚠️ Troubleshooting

### Error: `Table 'pizzas' doesn't exist`

Create/import the `pizzas` table before executing the analysis queries.

### Error: `Table 'pizza_types' doesn't exist`

Create/import the `pizza_types` table containing `pizza_type_id`, `name`, and `category`.

### Error with `HOUR()`

Make sure `order_time` is stored using the MySQL `TIME` data type.

### Incorrect Revenue Results

Check that:

* `quantity` contains valid numeric values.
* `price` contains the correct pizza price.
* `pizza_id` correctly connects `order_details` and `pizzas`.
* `pizza_type_id` correctly connects `pizzas` and `pizza_types`.

---

# 📜 License

This project is intended for educational, portfolio, and SQL practice purposes.

```
```
