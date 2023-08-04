# Model-Car-Database-with-MySQL
Analyze Data in a Model Car Database with MySQL Workbench

-- Analyze Data in a Model Car Database 
-- DATA ANALYSIS AND EXPLORATION USING SQL (MYSQL WORKBENCH)
-- SQL Query

-- ---------------------------------------------------------
-- Customer Analysis
-- ---------------------------------------------------------
-- 01: Distinct Countries from Customers
-- Description: This SQL query retrieves a list of distinct countries from the "Customers" table.

SELECT DISTINCT country
FROM Customers;

-- 02:Total Customer Base Analysis
-- Description: This SQL query calculates the total number of distinct customers in the "Customers" table.

SELECT COUNT(DISTINCT customerNumber) AS Total_Customer_Base
FROM Customers;

-- -------------------------------------------
-- Order
-- -------------------------------------------

-- 3: Analysis of Orders by Country
-- Description:This SQL query analyzes orders by country, calculating the count of orders for each country.
-- It joins the "orders" and "customers" tables based on customer numbers, grouping the results by country.

SELECT country, COUNT(country) AS count
FROM orders o
JOIN customers c
ON o.customerNumber = c.customerNumber
GROUP BY country
ORDER BY count DESC;

-- 04: Average Quantity Ordered and Average Price Each Analysis
-- Description:This SQL query calculates the average quantity ordered and the average price each for products in the "orderdetails" table.
-- It uses the `AVG` function twice to compute both the average quantity ordered and the average price each.

SELECT AVG(quantityOrdered) AS Avg_Quantity_Ordered,
       AVG(priceEach) AS Avg_Price_Each
FROM orderdetails;

-- 05: Top 10 Products by Total Quantity Sold Analysis
-- Description: This SQL query identifies the top 10 products with the highest total quantity sold.
-- It selects the product code and calculates the sum of quantities ordered from the "orderdetails" table.
-- The results are grouped by product code and ordered in descending order based on the total quantity sold.

SELECT productCode, SUM(quantityOrdered) AS total_quantity_sold
FROM orderdetails
GROUP BY productCode
ORDER BY total_quantity_sold DESC
LIMIT 10;

-- 06: Order Status Distribution Analysis
-- Description: This SQL query calculates the distribution of orders by different statuses.
-- It selects the order status and uses the COUNT function to count the occurrences of each status.
-- The results are grouped by status, providing insights into the distribution of orders based on their statuses.

SELECT status, COUNT(*) AS order_count
FROM orders
GROUP BY status;

-- 07:Pending Orders Analysis
-- Description: This SQL query retrieves a list of orders that are not yet shipped or completed.
-- It selects the order number, order date, and status from the "orders" table.
-- The WHERE clause filters orders that have statuses other than "Shipped" and "Completed."

SELECT orderNumber, orderDate, status
FROM orders
WHERE status NOT IN ('Shipped', 'Completed');

-- 08: Shipped Orders Analysis
-- Description: This SQL query retrieves specific details of orders that have been marked as "Shipped".
-- It selects order numbers, customer numbers, shipped dates, and order statuses from the "orders" table.
-- The results are filtered to show only orders with the "Shipped" status and are ordered by customer number.

SELECT orderNumber, customerNumber, shippedDate, status
FROM orders
WHERE status = "Shipped"
ORDER BY customerNumber;

-- 09: Count of Shipped Orders Analysis
-- Description: This SQL query calculates the total count of orders that have been marked as "Shipped".
-- It uses the COUNT function to count the occurrences of "Shipped" status in the "orders" table.

SELECT COUNT(status) AS total_of_shipped
FROM orders
WHERE status = "Shipped";

-- 10: Order and Product Details Analysis
-- Description: This SQL query retrieves comprehensive details about ordered products and associated orders.
-- It joins data from the "orderdetails," "products," and "orders" tables using common columns.
-- The result includes product code, order number, order date, product name, product line, quantity ordered, unit price, and calculated total price.

SELECT od.productCode,
       od.orderNumber,
       o.orderDate,
       p.productName,
       p.productLine,
       od.quantityOrdered,
       od.priceEach,
       (od.quantityOrdered * od.priceEach) AS Total_Price
FROM orderdetails od
JOIN products p
USING (productCode)
JOIN orders o
USING (orderNumber)
ORDER BY orderNumber;

-- 11. Customer Order Count Analysis
-- -----------------------------
-- Query: This SQL query calculates the order count for each customer and arranges the results in descending order by order count.
-- It selects the customer number and uses the COUNT function to count the number of orders made by each customer.

SELECT customerNumber, COUNT(orderNumber) AS order_count
FROM orders
GROUP BY customerNumber
ORDER BY order_count DESC;

-- -------------------------------------------------
-- Employee
-- ------------------------------------------------

-- 12: Employee and Supervisor Information
-- Query: This SQL query retrieves information about employees and their corresponding supervisors.
-- It uses a self-join on the "employees" table, associating employees with their supervisors by matching "ReportsTo" values.

SELECT e.employeeNumber,
       CONCAT(e.firstName, ' ', e.lastName) AS Employee_Name,
       CONCAT(em.firstName, ' ', em.lastName) AS Supervisor_Name
FROM employees e
JOIN employees em ON e.ReportsTo = em.employeeNumber;

-- 13: Employee and Office Details
-- Query Description:
-- This SQL query retrieves information about employees along with their corresponding office details.
-- It performs a join between the "employees" and "offices" tables based on the "officeCode" column.

SELECT
    e.employeeNumber,          -- Unique identifier of each employee.
    e.FirstName,               -- First name of each employee.
    e.LastName,                -- Last name of each employee.
    e.jobTitle,                -- Job title of each employee.
    o.city,                    -- City of the office where the employee works.
    o.addressLine1  AS  address, -- First line of the office address, renamed to "address".
    o.state,                   -- State of the office location.
    o.country                  -- Country of the office location.
FROM
    employees e                -- Alias for the "employees" table.
JOIN
    offices o                  -- Alias for the "offices" table.
ON
    e.officeCode = o.officeCode -- Joining based on the office code of employees and offices.
ORDER BY
    employeeNumber;            -- Results arranged in ascending order by employee number.

-- ---------------------------------------------
-- Payments
-- --------------------------------------------
-- 14: Payments Before 2003-12-31 Analysis
-- Query Description:
-- This SQL query retrieves payment information made by customers before or on December 31, 2003.
-- It selects customer numbers, payment dates, and payment amounts from the "payments" table.

SELECT customerNumber,
       paymentDate,
       amount
FROM payments
WHERE paymentDate <= '2003-12-31';

-- 15:Total Sales in 2003 Analysis
-- Query Description:
-- This SQL query calculates the total sales amount for transactions made before or on December 31, 2003.
-- It uses the SUM function to aggregate payment amounts from the "payments" table.
-- The result provides insights into the total revenue generated within the specified time frame.

SELECT SUM(amount) AS Total_sales_2003,
       COUNT(amount) AS number_of_payments
FROM payments
WHERE paymentDate <= '2003-12-31';

-- 16:Payments in 2004 Analysis
-- ---------------------------------------
-- Query Description:
-- This SQL query retrieves payment details made by customers during the year 2004.
-- It selects customer numbers, payment dates, and payment amounts from the "payments" table.

SELECT customerNumber, paymentDate, amount
FROM payments
WHERE paymentDate BETWEEN '2004-01-01' AND '2004-12-31';

-- Query Description:
-- This SQL query calculates the total sales amount and the count of sales transactions for the year 2004.
-- It aggregates payment amounts and counts payment records from the "payments" table.

SELECT SUM(amount) AS total_sales_2004,
       COUNT(amount) AS sales_Count_2004
FROM payments
WHERE paymentDate BETWEEN '2004-01-01' AND '2004-12-31';

-- 17:Payments in 2005 with Amount Analysis
-- -----------------------------------------------
-- Query Description:
-- This SQL query retrieves payment details made by customers during the year 2005, ordered by payment amount in descending order.
-- It selects customer numbers, payment dates, and payment amounts from the "payments" table.

SELECT customerNumber, paymentDate, amount
FROM payments
WHERE paymentDate BETWEEN '2005-01-01' AND '2005-12-31'
ORDER BY amount DESC;

-- Query Description:
-- This SQL query calculates the count of payments made by customers during the year 2005.
-- It uses the COUNT function to tally the number of payment records within the specified date range.

SELECT COUNT(amount) AS number_of_payments
FROM payments
WHERE paymentDate BETWEEN '2005-01-01' AND '2005-12-31';

-- 18:Product Sales Analysis by Product Line
-- --------------------------------------
-- Query Description:
-- This SQL query counts the number of sales for each product line and presents the results in descending order.
-- It retrieves product line information from the "products" table and uses the COUNT function to count sales transactions.
-- The results are grouped by product line, providing insights into the popularity of different product lines.

SELECT p.productLine,
       COUNT(od.productCode) AS num_of_sales
FROM products p
JOIN orderdetails od
ON p.productCode = od.productCode
GROUP BY p.productLine
ORDER BY num_of_sales DESC;


