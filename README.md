# 🚗 Model Car Database Analysis with MySQL

Analyze business performance, customer behavior, and sales trends using SQL queries on a model car database.

---

## 📌 Project Overview

This project focuses on **data exploration and business analysis** using MySQL Workbench.  
It answers key business questions related to:

- Customer distribution 🌍  
- Order patterns 📦  
- Sales performance 💰  
- Employee hierarchy 👨‍💼  
- Payment trends 📊  

The goal is to simulate **real-world business intelligence analysis using SQL**.

---

## 🛠️ Tools & Technologies

- MySQL Workbench  
- SQL (Joins, Aggregations, Filtering, Grouping)  
- Relational Database Analysis  

---

## 📂 Dataset Description

The database includes the following tables:

- `customers`
- `orders`
- `orderdetails`
- `products`
- `employees`
- `offices`
- `payments`

---

## 📊 Key Analysis Performed

**👥 Customer Analysis**

```sql
SELECT COUNT(DISTINCT customerNumber) AS Total_Customer_Base
FROM Customers;
SELECT DISTINCT country FROM Customers;

**📦 Order Analysis**

SELECT country, COUNT(*) AS count
FROM orders o
JOIN customers c
ON o.customerNumber = c.customerNumber
GROUP BY country
ORDER BY count DESC;

**🛍️ Product & Sales Analysis**

SELECT productCode, SUM(quantityOrdered) AS total_quantity_sold
FROM orderdetails
GROUP BY productCode
ORDER BY total_quantity_sold DESC
LIMIT 10;

**💳 Payment Analysis**

SELECT SUM(amount) AS total_sales_2004,
       COUNT(amount) AS sales_Count_2004
FROM payments
WHERE paymentDate BETWEEN '2004-01-01' AND '2004-12-31';

**👨‍💼 Employee Analysis**

SELECT e.employeeNumber,
       CONCAT(e.firstName, ' ', e.lastName) AS Employee_Name,
       CONCAT(em.firstName, ' ', em.lastName) AS Supervisor_Name
FROM employees e
JOIN employees em ON e.ReportsTo = em.employeeNumber;

📈 Business Impact

This analysis helps:

Identify high-performing products
Optimize inventory & supply chain
Improve customer targeting
Track revenue trends
Enable data-driven decision making

⭐ If you found this useful

Give this repo a ⭐ and connect with me!
