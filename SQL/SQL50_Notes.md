# SQL 50 Notes

## Problem 1: Recyclable and Low Fat Products

Concepts:

* SELECT
* WHERE
* AND

Query:
SELECT product_id
FROM Products
WHERE low_fats = 'Y'
AND recyclable = 'Y';

---

## Problem 2: Find Customer Referee

Concepts:

* <>
* IS NULL

Query:
SELECT name
FROM Customer
WHERE referee_id <> 2
OR referee_id IS NULL;

---

## Problem 3: Big Countries

Concepts:

* OR
* Numeric filtering

Query:
SELECT name, population, area
FROM World
WHERE area >= 3000000
OR population >= 25000000;

---

## Problem 4: Article Views I

Concepts:

* DISTINCT
* AS (Alias)

Query:
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY author_id;

---

## Problem 5: Invalid Tweets

Concepts:

* LENGTH()

Query:
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15;

## Problem 6: Replace Employee ID With The Unique Identifier

Concepts:

* LEFT JOIN

Query:
SELECT unique_id, name
FROM Employees
LEFT JOIN EmployeeUNI
ON Employees.id = EmployeeUNI.id;

---

## Problem 7: Product Sales Analysis I

Concepts:

* INNER JOIN

Query:
SELECT product_name, year, price
FROM Sales
INNER JOIN Product
ON Sales.product_id = Product.product_id;

---

## Problem 8: Customer Who Visited but Did Not Make Any Transactions

Concepts:

* LEFT JOIN
* IS NULL
* COUNT()
* GROUP BY

Query:
SELECT customer_id,
COUNT(Visits.visit_id) AS count_no_trans
FROM Visits
LEFT JOIN Transactions
ON Visits.visit_id = Transactions.visit_id
WHERE transaction_id IS NULL
GROUP BY customer_id;

Learning:
When two tables contain the same column name, specify the table name (e.g., Visits.visit_id).

