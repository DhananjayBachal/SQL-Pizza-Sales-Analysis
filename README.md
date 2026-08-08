# 🍕 Pizza Sales Analysis – SQL Project

## 📌 Project Overview

This project analyzes **pizza sales data using MySQL** to uncover valuable business insights such as total orders, revenue, popular pizza sizes, top-selling pizzas, category performance, hourly order trends, and revenue contribution.

The project demonstrates practical SQL skills including **JOINs, GROUP BY, aggregate functions, subqueries, window functions, CTE-style analysis, ranking, and data aggregation**.

---

## 🎯 Project Objectives

* Analyze overall pizza sales performance
* Calculate total orders and revenue
* Identify the most popular pizza sizes and types
* Find top-selling pizzas based on quantity and revenue
* Analyze pizza category performance
* Understand hourly order patterns
* Calculate average pizzas ordered per day
* Analyze revenue contribution by category
* Track cumulative revenue over time
* Identify top 3 pizzas within each category

---

## 🛠️ Technologies Used

* **MySQL**
* **SQL**
* **MySQL Workbench**
* **Git & GitHub**

---

## 🗂️ Database Structure

The analysis uses the following tables:

### `orders`

| Column       | Description             |
| ------------ | ----------------------- |
| `order_id`   | Unique order identifier |
| `order_date` | Date of the order       |
| `order_time` | Time of the order       |

### `order_details`

| Column             | Description                    |
| ------------------ | ------------------------------ |
| `order_details_id` | Unique order-detail identifier |
| `order_id`         | Reference to the order         |
| `pizza_id`         | Reference to the pizza         |
| `quantity`         | Number of pizzas ordered       |

### Additional Tables

* `pizzas`
* `pizza_types`

These tables provide pizza pricing, size, category, and pizza type information.

---

## 🔍 SQL Analysis Performed

### 1. Total Number of Orders

Calculated the total number of orders placed using `COUNT()`.

### 2. Total Revenue

Calculated total sales revenue using:

`quantity × pizza price`

and rounded the result using `ROUND()`.

### 3. Highest-Priced Pizza

Identified the pizza with the highest price using `ORDER BY` and `LIMIT`.

### 4. Most Common Pizza Size

Analyzed order details to determine the most frequently ordered pizza size.

### 5. Top 5 Most Ordered Pizza Types

Used `SUM()`, `GROUP BY`, and `ORDER BY` to identify the five pizza types with the highest quantities sold.

### 6. Pizza Category Performance

Analyzed the total quantity of pizzas ordered for each category.

### 7. Orders by Hour

Used the MySQL `HOUR()` function to determine the distribution of orders throughout the day.

### 8. Category-Wise Pizza Distribution

Analyzed the number of pizza types available in each category.

### 9. Average Daily Pizza Orders

Calculated the average number of pizzas ordered per day using a subquery and aggregation.

### 10. Top 3 Pizzas by Revenue

Identified the three pizza types generating the highest revenue.

### 11. Revenue Contribution

Calculated the percentage contribution of each pizza category to total revenue.

### 12. Cumulative Revenue Analysis

Used the SQL **window function `SUM() OVER()`** to calculate cumulative revenue over time.

### 13. Top 3 Pizzas by Category

Used the **`RANK()` window function** with `PARTITION BY` to identify the top three revenue-generating pizzas within each category.

---

## 💡 Key SQL Concepts Demonstrated

```text
✓ SELECT
✓ WHERE
✓ COUNT()
✓ SUM()
✓ AVG()
✓ ROUND()
✓ GROUP BY
✓ ORDER BY
✓ LIMIT
✓ INNER JOIN
✓ Subqueries
✓ Aggregate Functions
✓ Window Functions
✓ SUM() OVER()
✓ RANK()
✓ PARTITION BY
✓ Date & Time Functions
✓ Business KPI Analysis
```

---

## 📊 Business Questions Answered

This project answers important business questions such as:

* How many orders were placed?
* What is the total revenue generated?
* Which pizza has the highest price?
* What is the most popular pizza size?
* Which pizzas are ordered the most?
* Which pizza category has the highest demand?
* What time of day receives the most orders?
* What is the average number of pizzas ordered per day?
* Which pizzas generate the most revenue?
* What percentage of revenue comes from each category?
* How does cumulative revenue grow over time?
* Which are the top-performing pizzas within each category?

---

## 📁 Project Structure

```text
Pizza-Sales-Analysis/
│
├── Pizza Sales Analysis.sql
└── README.md
```

---

## 🚀 How to Run the Project

### 1. Clone the Repository

```bash
git clone https://github.com/DhananjayBachal
```

### 2. Open MySQL Workbench

Open the SQL file in **MySQL Workbench**.

### 3. Create the Database

```sql
CREATE DATABASE pizzahut;
USE pizzahut;
```

### 4. Load the Required Dataset

Import the required pizza sales tables:

```text
orders
order_details
pizzas
pizza_types
```

### 5. Execute the Queries

Run the SQL queries provided in the project file to reproduce the analysis.

---

## 📈 Skills Demonstrated

This project demonstrates my ability to:

* Write efficient SQL queries
* Perform relational database analysis
* Combine multiple tables using JOINs
* Analyze sales and revenue KPIs
* Perform aggregation and grouping
* Use subqueries for advanced analysis
* Apply SQL window functions
* Rank business performance metrics
* Extract actionable business insights from data

---

## 👨‍💻 Author

**Dhananjay Bachal**

🎓 Data Analytics Enthusiast
💻 SQL | Python | Excel | Data Analytics

### Connect With Me

* GitHub: https://github.com/DhananjayBachal
* LinkedIn: https://www.linkedin.com/in/dhananjay-bachal/

---

## ⭐ If You Find This Project Useful

Feel free to **star ⭐ the repository** and explore my other data analytics projects.
