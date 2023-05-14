## This is a set of MySQL assessment test questions, including sample code snippets and queries that you can use to evaluate the skills of job candidates.

# Prerequisites
```sqlsql
CREATE TABLE customers (
  customer_id INT PRIMARY KEY AUTO_INCREMENT,
  first_name VARCHAR(50) NOT NULL,
  last_name VARCHAR(50) NOT NULL,
  email VARCHAR(100) NOT NULL UNIQUE,
  phone VARCHAR(20),
  address VARCHAR(100),
  city VARCHAR(50),
  state VARCHAR(50),
  zip_code VARCHAR(20)
);

INSERT INTO customers (customer_id, first_name, last_name, email, phone, address, city, state, zip_code)
VALUES
  (1, 'John', 'Doe', 'johndoe@gmail.com', '555-1234', '123 Main St', 'Anytown', 'CA', '12345'),
  (2, 'Jane', 'Smith', 'janesmith@yahoo.com', '555-5678', '456 Oak Ave', 'Somecity', 'NY', '67890'),
  (3, 'Bob', 'Johnson', 'bob.johnson@hotmail.com', '555-4321', '789 Elm St', 'Othercity', 'TX', '54321'),
  (4, 'Alice', 'Brown', 'abrown@gmail.com', '555-8765', '321 Pine St', 'Smalltown', 'IL', '98765');
  
CREATE TABLE orders (
  order_id INT PRIMARY KEY AUTO_INCREMENT,
  customer_id INT NOT NULL,
  order_date DATETIME NOT NULL,
  total DECIMAL(10,2) NOT NULL,
  FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

INSERT INTO orders (customer_id, order_date, total)
VALUES
  (1, '2022-05-01 09:00:00', 50.00),
  (1, '2022-05-07 12:30:00', 100.00),
  (2, '2022-05-10 16:45:00', 715 00),
  (3, '2022-05-12 08:15:00', 200.00),
  (4, '2022-05-13 11:00:00', 124 00),
  (2, '2022-06-13 14:30:00', 50.00),
  (1, '2022-07-14 10:00:00', 776 00),
  (4, '2022-08-14 13:45:00', 150.00);

CREATE TABLE order_items (
  order_item_id INT AUTO_INCREMENT,
  order_id INT NOT NULL,
  product_id INT NOT NULL,
  quantity INT NOT NULL,
  price DECIMAL(10,2) NOT NULL,
  discount DECIMAL(4,2) DEFAULT 0.00,
  PRIMARY KEY (order_item_id),
  FOREIGN KEY (order_id) REFERENCES orders(order_id),
  FOREIGN KEY (product_id) REFERENCES products(product_id)
);

INSERT INTO order_items (order_id, product_id, quantity, price, discount) VALUES
(1, 2, 1, 12.99, 0.00),
(1, 5, 2, 223.99, 0.10),
(2, 1, 3, 111.99, 0.00),
(2, 4, 1, 332.99, 0.05),
(3, 6, 2, 3.99, 0.00),
(4, 3, 1, 442.99, 0.00),
(5, 7, 3, 254.99, 0.15),
(5, 8, 1, 53.99, 0.00);


CREATE TABLE products (
  product_id INT PRIMARY KEY AUTO_INCREMENT,
  product_name VARCHAR(255) NOT NULL,
  category_id INT NOT NULL,
  unit_price DECIMAL(10, 2) NOT NULL,
  description TEXT,
  CONSTRAINT fk_category
    FOREIGN KEY (category_id)
    REFERENCES categories(category_id)
    ON DELETE CASCADE
);

INSERT INTO products (product_name, category_id, unit_price, description) VALUES
('Product 1', 1, 10.00, 'This is product 1'),
('Product 2', 1, 1### 00, 'This is product 2'),
('Product 3', 2, 20.00, 'This is product 3'),
('Product 4', 2, 2### 00, 'This is product 4'),
('Product 5', 3, 30.00, 'This is product 5'),
('Product 6', 3, 3### 00, 'This is product 6');

CREATE TABLE categories (
  category_id INT PRIMARY KEY AUTO_INCREMENT,
  category_name VARCHAR(255) NOT NULL
);

INSERT INTO categories (category_name) VALUES
('Category 1'),
('Category 2'),
('Category 3');
```

### Write a MySQL query to display the names and total sales of all customers who have placed an order for a product with a discount greater than 10%.

```sql
SELECT customers.first_name, customers.last_name, SUM(orders.total) AS total_sales
FROM customers
INNER JOIN orders ON customers.customer_id = orders.customer_id
INNER JOIN order_items ON orders.order_id = order_items.order_id
INNER JOIN products ON order_items.product_id = products.product_id
WHERE order_items.discount > 0.10
GROUP BY customers.customer_id;
```


### Write a MySQL query to display the names and total sales of the top 10% of customers, based on the total sales amount.

```sql
SELECT customers.first_name, customers.last_name, SUM(orders.total) AS total_sales
FROM customers
INNER JOIN orders ON customers.customer_id = orders.customer_id
GROUP BY customers.customer_id
HAVING total_sales >= (
  SELECT 0.1 * SUM(orders.total) AS top_sales
  FROM orders
)
ORDER BY total_sales DESC;
```

### Write a MySQL query to display the names and total sales of all customers who have placed an order in the month of January.

```sql
SELECT customers.first_name, customers.last_name, SUM(orders.total) AS total_sales
FROM customers
INNER JOIN orders ON customers.customer_id = orders.customer_id
WHERE MONTH(orders.order_date) = 1
GROUP BY customers.customer_id;
```

### Write a MySQL query to display the names and email addresses of all customers who have not placed an order.

```sql
SELECT customers.first_name, customers.last_name, customers.email
FROM customers
WHERE customers.customer_id NOT IN (
  SELECT orders.customer_id
  FROM orders
);
```

###  Write a MySQL query to display the names and email addresses of all customers who have placed an order for a product with a unit price greater than $50.

```sql
SELECT customers.first_name, customers.last_name, customers.email
FROM customers
INNER JOIN orders ON customers.customer_id = orders.customer_id
INNER JOIN order_items ON orders.order_id = order_items.order_id
INNER JOIN products ON order_items.product_id = products.product_id
WHERE products.unit_price > 50.00;
```

### Write a MySQL query to display the total number of orders and the average order total for each month in the current year.

```sql
SELECT MONTH(orders.order_date) AS month, COUNT(orders.order_id) AS total_orders, AVG(orders.total) AS avg_order_total
FROM orders
WHERE YEAR(orders.order_date) = YEAR(CURDATE())
GROUP BY MONTH(orders.order_date);
```

### Write a MySQL query to display the names and email addresses of all customers 
who have placed an order for a product in the "clothing" category, 
but have not placed an order for a product in the "electronics" category.

```sql
SELECT customers.first_name, customers.last_name, customers.email
FROM customers
INNER JOIN orders ON customers.customer_id = orders.customer_id
INNER JOIN order_items ON orders.order_id = order_items.order_id
INNER JOIN products ON order_items.product_id = products.product_id
INNER JOIN categories ON products.category_id = categories.category_id
WHERE categories.category_name = 'clothing'
AND customers.customer_id NOT IN (
  SELECT customers.customer_id
  FROM customers
  INNER JOIN orders ON customers.customer_id = orders.customer_id
  INNER JOIN order_items ON orders.order_id = order_items.order_id
  INNER JOIN products ON order_items.product_id = products.product_id
  INNER JOIN categories ON products.category_id = categories.category_id
  WHERE categories.category_name = 'electronics'
);
```

### Write a MySQL query to display the names and email addresses of all customers who have placed at least two orders.

```sql
SELECT customers.first_name, customers.last_name, customers.email
FROM customers
INNER JOIN orders ON customers.customer_id = orders.customer_id
GROUP BY customers.customer_id
HAVING COUNT(orders.order_id) >= 2;
```


### Write a MySQL query to display the names and total sales of all customers who have placed an order for a product with a unit price greater than $100.

```sql
SELECT customers.first_name, customers.last_name, SUM(orders.total) AS total_sales
FROM customers
INNER JOIN orders ON customers.customer_id = orders.customer_id
INNER JOIN order_items ON orders.order_id = order_items.order_id
INNER JOIN products ON order_items.product_id = products.product_id
WHERE products.unit_price > 100.00
GROUP BY customers.customer_id;
```

### Write a MySQL query to display the total number of orders for each customer.

```sql
SELECT customers.customer_id, customers.first_name, customers.last_name, COUNT(orders.order_id) AS total_orders
FROM customers
LEFT JOIN orders ON customers.customer_id = orders.customer_id
GROUP BY customers.customer_id;
```

###  Write a MySQL query to display the top 5 customers who have placed the most orders.

```sql
SELECT customers.customer_id, customers.first_name, customers.last_name, COUNT(orders.order_id) AS total_orders
FROM customers
LEFT JOIN orders ON customers.customer_id = orders.customer_id
GROUP BY customers.customer_id
ORDER BY total_orders DESC
LIMIT 5;
```

###  Write a MySQL query to add a new column called "phone" to the "customers" table.

```sql
ALTER TABLE customers ADD COLUMN phone VARCHAR(20);
```

###  Write a MySQL query to update the "email" column for the customer with customer_id = 123. 

```sql
UPDATE customers SET email = 'new_email@example.com' WHERE customer_id = 123;
```

###  Write a MySQL query to delete all orders that were placed before January 1, 2022.

```sql
DELETE FROM orders WHERE order_date < '2022-01-01';
```

###  Write a MySQL query to display the average order total for each month in 2022.

```sql
SELECT DATE_FORMAT(order_date, '%Y-%m') AS month, AVG(total) AS avg_total
FROM orders
WHERE order_date BETWEEN '2022-01-01' AND '2022-12-31'
GROUP BY month;
```

### Write a MySQL query to display the total orders for each customer in the "customers" table, sorted in descending order by total sales.
```sql
SELECT customers.customer_id, customers.first_name, customers.last_name, sum(orders.total) as sum_orders
FROM customers
LEFT JOIN orders ON customers.customer_id = orders.customer_id
GROUP BY customers.customer_id
ORDER BY sum_orders DESC;
```

### Write a MySQL query to display the names and email addresses of all customers who have placed an order in the last 30 days.

```sql
SELECT customers.first_name, customers.last_name, customers.email
FROM customers
INNER JOIN orders ON customers.customer_id = orders.customer_id
WHERE orders.order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY);
```
**`DATE_SUB(NOW(), INTERVAL 30 DAY)` is a MySQL function that subtracts 30 days from the current date and returns the resulting date as a string in the format "YYYY-MM-DD".**


###  Write a MySQL query to display the total number of orders and the average order total for each customer, sorted in descending order by the average order total.

```sql
SELECT customers.customer_id, customers.first_name, customers.last_name, COUNT(orders.order_id) AS total_orders, AVG(orders.total) AS avg_order_total
FROM customers
LEFT JOIN orders ON customers.customer_id = orders.customer_id
GROUP BY customers.customer_id
ORDER BY avg_order_total DESC;
```

###  Write a MySQL query to display the names and email addresses of all customers who have not placed an order in the last 90 days.

```sql
SELECT customers.first_name, customers.last_name, customers.email
FROM customers
LEFT JOIN orders ON customers.customer_id = orders.customer_id
WHERE orders.order_date < DATE_SUB(NOW(), INTERVAL 90 DAY) OR orders.order_date IS NULL;
```

###  Write a MySQL query to display the top 5 best-selling products in each category, based on the total quantity sold.

```sql
SELECT categories.category_name, products.product_name, SUM(order_items.quantity) AS total_quantity_sold
FROM categories
LEFT JOIN products ON categories.category_id = products.category_id
LEFT JOIN order_items ON products.product_id = order_items.product_id
GROUP BY categories.category_id, products.product_id
HAVING total_quantity_sold > 0
ORDER BY categories.category_name, total_quantity_sold DESC
LIMIT 5;
```

### Write a MySQL query to display the second highest salary from an employees:
```sqlsql

CREATE TABLE employees (
  employee_id INT PRIMARY KEY AUTO_INCREMENT,
  first_name VARCHAR(50) NOT NULL,
  last_name VARCHAR(50) NOT NULL,
  email VARCHAR(100) NOT NULL,
  phone_number VARCHAR(20),
  hire_date DATE NOT NULL,
  job_id VARCHAR(10) NOT NULL,
  salary DECIMAL(8,2),
  commission_pct DECIMAL(2,2),
  manager_id INT,
  department_id INT
);


INSERT INTO employees (first_name, last_name, email, phone_number, hire_date, job_id, salary, commission_pct, manager_id, department_id) VALUES
('John', 'Doe', 'johndoe@example.com', '123-456-7890', '2020-01-01', 'IT_PROG', 60000.00, NULL, NULL, 60),
('Jane', 'Smith', 'janesmith@example.com', '234-567-8901', '2021-03-15', 'SA_REP', 80000.00, 0.10, 1, 80),
('Bob', 'Jones', 'bobjones@example.com', '345-678-9012', '2019-05-01', 'IT_PROG', 55000.00, NULL, 1, 60),
('Mary', 'Johnson', 'maryjohnson@example.com', '456-789-0123', '2018-10-15', 'HR_REP', 65000.00, NULL, 2, 50),
('Tom', 'Williams', 'tomwilliams@example.com', '567-890-1234', '2017-06-30', 'SA_MAN', 120000.00, 0.20, 1, 80),
('Sara', 'Garcia', 'saragarcia@example.com', '678-901-2345', '2022-02-28', 'IT_PROG', 50000.00, NULL, 3, 60),
('Adam', 'Lee', 'adamlee@example.com', '789-012-3456', '2016-01-01', 'SA_REP', 75000.00, 0.05, 5, 80),
('Emily', 'Chen', 'emilychen@example.com', '890-123-4567', '2021-09-01', 'IT_PROG', 45000.00, NULL, 3, 60);


SELECT MAX(salary) FROM employees WHERE salary < (SELECT MAX(salary) FROM employees);
-- OR
SELECT salary FROM employees ORDER BY salary DESC LIMIT 1 OFFSET 0; -- the first highest salary
SELECT salary FROM employees ORDER BY salary DESC LIMIT 1 OFFSET 1; -- the second highest salary
SELECT salary FROM employees ORDER BY salary DESC LIMIT 1 OFFSET 2; -- the third highest salary
SELECT salary FROM employees ORDER BY salary DESC LIMIT 1 OFFSET 3; -- the forth highest salary
```


###  Write a query to find the total number of orders placed in the last month.

```sql
SELECT COUNT(*) FROM orders
WHERE order_date >= DATE_SUB(NOW(), INTERVAL 1 MONTH);
```

### Write a query to find the top 5 customers with the highest total order amount.

```sql
SELECT customers.customer_name, SUM(order_details.quantity * order_details.unit_price) AS total_order_amount
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id
JOIN order_details ON orders.order_id = order_details.order_id
GROUP BY customers.customer_id
ORDER BY total_order_amount DESC
LIMIT 5;
```

###  Write a query to find the products that have been ordered more than 100 times.

```sql
SELECT products.product_name, COUNT(order_details.order_id) AS order_count
FROM products
JOIN order_details ON products.product_id = order_details.product_id
GROUP BY products.product_id
HAVING order_count > 100;
```

###  Write a query to find the average time it takes for an order to be shipped.

```sql
SELECT AVG(DATEDIFF(shipped_date, order_date)) AS avg_shipping_time
FROM orders
WHERE shipped_date IS NOT NULL;
```

###  Write a query to find the customers who have not placed any orders in the last 6 months.

```sql
SELECT customers.customer_name
FROM customers
LEFT JOIN orders ON customers.customer_id = orders.customer_id
WHERE orders.order_date < DATE_SUB(NOW(), INTERVAL 6 MONTH) OR orders.order_date IS NULL;
```

Sure, here are some more advanced sample MySQL queries for an assessment test:

###  Write a query to find the top 3 best-selling categories, along with the total revenue generated by each category.

```sql
SELECT categories.category_name, SUM(order_details.quantity * order_details.unit_price) AS total_revenue
FROM categories
JOIN products ON categories.category_id = products.category_id
JOIN order_details ON products.product_id = order_details.product_id
GROUP BY categories.category_id
ORDER BY total_revenue DESC
LIMIT 3;
```

### Write a query to find the average revenue per customer, broken down by country.

```sql
SELECT customers.country, AVG(order_details.quantity * order_details.unit_price) AS avg_revenue_per_customer
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id
JOIN order_details ON orders.order_id = order_details.order_id
GROUP BY customers.country;
```

###  Write a query to find the top 5 most profitable products, along with the profit margin percentage for each product.

```sql
SELECT products.product_name, (SUM(order_details.quantity * order_details.unit_price) - SUM(order_details.quantity * products.unit_cost)) / SUM(order_details.quantity * order_details.unit_price) * 100 AS profit_margin_percentage
FROM products
JOIN order_details ON products.product_id = order_details.product_id
GROUP BY products.product_id
ORDER BY profit_margin_percentage DESC
LIMIT 5;
```

###  Write a query to find the customers who have made at least 10 orders in the past year, along with the total amount they have spent.

```sql
SELECT customers.customer_name, SUM(order_details.quantity * order_details.unit_price) AS total_spent
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id
JOIN order_details ON orders.order_id = order_details.order_id
WHERE orders.order_date >= DATE_SUB(NOW(), INTERVAL 1 YEAR)
GROUP BY customers.customer_id
HAVING COUNT(DISTINCT orders.order_id) >= 10;
```

###  Write a query to find the products that have been ordered by at least 5 different customers, along with the total quantity ordered.

```sql
SELECT products.product_name, SUM(order_details.quantity) AS total_quantity_ordered
FROM products
JOIN order_details ON products.product_id = order_details.product_id
JOIN orders ON order_details.order_id = orders.order_id
WHERE orders.customer_id IN (
  SELECT customer_id
  FROM orders
  GROUP BY customer_id
  HAVING COUNT(DISTINCT order_id) >= 5
)
GROUP BY products.product_id;
```

###  Write a query to find the top 5 most loyal customers, based on the number of orders they have placed.

```sql
SELECT customers.customer_name, COUNT(orders.order_id) AS total_orders
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id
GROUP BY customers.customer_id
ORDER BY total_orders DESC
LIMIT 5;
```

### Write a query to find the average time it takes for a customer to place a repeat order.

```sql
SELECT AVG(DATEDIFF(
  (SELECT MIN(order_date) FROM orders AS o WHERE o.customer_id = orders.customer_id AND o.order_date > orders.order_date),
  orders.order_date
)) AS avg_time_to_repeat_order
FROM orders
WHERE (
  SELECT COUNT(*)
  FROM orders AS o
  WHERE o.customer_id = orders.customer_id AND o.order_date < orders.order_date
) > 0;
```

###  Write a query to find the top 3 countries where customers have the highest average order value.

```sql
SELECT customers.country, AVG(order_details.quantity * order_details.unit_price) AS avg_order_value
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id
JOIN order_details ON orders.order_id = order_details.order_id
GROUP BY customers.country
ORDER BY avg_order_value DESC
LIMIT 3;
```

###  Write a query to find the products that have been ordered by customers from at least 3 different countries.

```sql
SELECT products.product_name
FROM products
JOIN order_details ON products.product_id = order_details.product_id
JOIN orders ON order_details.order_id = orders.order_id
JOIN customers ON orders.customer_id = customers.customer_id
WHERE (
  SELECT COUNT(DISTINCT customers.country)
  FROM orders
  JOIN customers ON orders.customer_id = customers.customer_id
  WHERE orders.order_id = order_details.order_id
) >= 3
GROUP BY products.product_id;
```

###  Write a query to find the customers who have made at least 2 orders in each quarter of the past year.

```sql
SELECT customers.customer_name
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id
WHERE YEAR(orders.order_date) = YEAR(NOW()) - 1
GROUP BY customers.customer_id, QUARTER(orders.order_date)
HAVING COUNT(DISTINCT orders.order_id) >= 2
```

Sure, here are some even more advanced sample MySQL queries for an assessment test:

###  Write a query to find the total revenue generated for each month in the past year, broken down by product category.

```sql
SELECT categories.category_name, YEAR(order_details.order_date) AS year, MONTH(order_details.order_date) AS month, SUM(order_details.quantity * order_details.unit_price) AS total_revenue
FROM categories
JOIN products ON categories.category_id = products.category_id
JOIN order_details ON products.product_id = order_details.product_id
WHERE order_details.order_date >= DATE_SUB(NOW(), INTERVAL 1 YEAR)
GROUP BY categories.category_id, YEAR(order_details.order_date), MONTH(order_details.order_date)
ORDER BY categories.category_id, YEAR(order_details.order_date), MONTH(order_details.order_date);
```

### Write a query to find the top 10 customers who have made the most repeat orders, along with the number of repeat orders they have made.

```sql
SELECT customers.customer_name, COUNT(DISTINCT repeat_orders.order_id) AS total_repeat_orders
FROM customers
JOIN orders ON customers.customer_id = orders.customer_id
JOIN orders AS repeat_orders ON customers.customer_id = repeat_orders.customer_id AND repeat_orders.order_date > orders.order_date
GROUP BY customers.customer_id
ORDER BY total_repeat_orders DESC
LIMIT 10;
```

###  Write a query to find the top 5 products that have the highest customer lifetime value (CLV), based on the total amount spent by customers who have purchased the product.

```sql
SELECT products.product_name, SUM(order_details.quantity * order_details.unit_price) / COUNT(DISTINCT orders.customer_id) AS clv
FROM products
JOIN order_details ON products.product_id = order_details.product_id
JOIN orders ON order_details.order_id = orders.order_id
WHERE orders.customer_id IN (
  SELECT customer_id
  FROM orders
  JOIN order_details ON orders.order_id = order_details.order_id
  JOIN products ON order_details.product_id = products.product_id
  WHERE products.product_id = order_details.product_id
  GROUP BY customer_id
  HAVING COUNT(DISTINCT products.product_id) >= 2
)
GROUP BY products.product_id
ORDER BY clv DESC
LIMIT 5;
```

###  Write a query to find the customers who have placed orders on consecutive days, along with the date range of their consecutive orders.

```sql
SELECT orders1.customer_id, MIN(orders1.order_date) AS start_date, MAX(orders2.order_date) AS end_date
FROM orders AS orders1
JOIN orders AS orders2 ON orders1.customer_id = orders2.customer_id AND DATEDIFF(orders2.order_date, orders1.order_date) = 1
GROUP BY orders1.customer_id, orders1.order_date
HAVING COUNT(*) >= 2;
```

### Write a query to find the top 3 product categories that have the highest percentage of repeat customers, based on the number of customers who have placed orders in the category more than once.

```sql
SELECT categories.category_name, COUNT(DISTINCT repeat_customers.customer_id) / COUNT(DISTINCT customers.customer_id) * 100 AS repeat_customer_percentage
FROM categories
JOIN products ON categories.category_id = products.category_id
JOIN order_details ON products.product_id = order_details.product_id
JOIN orders ON order_details.order_id = orders.order_id
JOIN (
  SELECT customer_id, category_id, COUNT(DISTINCT orders.order_id) AS repeat_orders
  FROM orders
  JOIN order_details ON orders.order_id = order_details.order_id
  JOIN products ON order_details.product_id = products.product_id
  GROUP BY customer_id, category_id
  HAVING COUNT(DISTINCT orders.order_id) >= 2
```
