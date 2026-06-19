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
