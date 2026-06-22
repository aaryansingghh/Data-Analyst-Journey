# SQL 50 Notes

## Problem 1: Recyclable and Low Fat Products

Concepts:

* SELECT
* WHERE
* AND

Query:
Select product_id
from Products
where low_fats = 'Y'
and recyclable = 'Y';

---

## Problem 2: Find Customer Referee

Concepts:

* <>
* IS NULL

Query:
Select name
from Customer
where referee_id <> 2
or referee_id IS NULL;

---

## Problem 3: Big Countries

Concepts:

* OR
* Numeric filtering

Query:
Select name, population, area
from World
where area >= 3000000
or population >= 25000000;

---

## Problem 4: Article Views I

Concepts:

* DISTINCT
* AS (Alias)

Query:
Select DISTINCT author_id AS id
from Views
where author_id = viewer_id
order by author_id;

---

## Problem 5: Invalid Tweets

Concepts:

* LENGTH()

Query:
Select tweet_id
from Tweets
where LENGTH(content) > 15;

## Problem 6: Replace Employee ID With The Unique Identifier

Concepts:

* LEFT JOIN

Query:
Select unique_id, name
from Employees
left join EmployeeUNI
on Employees.id = EmployeeUNI.id;

---

## Problem 7: Product Sales Analysis I

Concepts:

* INNER JOIN

Query:
Select product_name, year, price
from Sales
inner join Product
on Sales.product_id = Product.product_id;

---

## Problem 8: Customer Who Visited but Did Not Make Any Transactions

Concepts:

* LEFT JOIN
* IS NULL
* COUNT()
* GROUP BY

Query:
Select customer_id,
count(Visits.visit_id) as count_no_trans
from Visits
left join Transactions
on Visits.visit_id = Transactions.visit_id
where transaction_id IS NULL
group by customer_id;

Learning:
When two tables contain the same column name, specify the table name (e.g., Visits.visit_id).

## Problem 9: Rising Temperature

Concepts:

* Self Join
* DATEDIFF()

Query:

Select w1.id
from Weather w1
join Weather w2
on DATEDIFF(w1.recordDate, w2.recordDate) = 1
where w1.temperature > w2.temperature;

Learning:

* Same table can be joined with itself.
* DATEDIFF() compares dates.
* Find days where temperature is higher than the previous day.

## Problem 10: Average Time of Process per Machine

Concepts:

* Self Join
* AVG()
* ROUND()
* GROUP BY

Query:

Select a.machine_id,round(avg(b.timestamp - a.timestamp), 3) as processing_time
from Activity a join Activity b
on a.machine_id = b.machine_id and a.process_id = b.process_id
where a.activity_type = 'start' and b.activity_type = 'end'
group by  a.machine_id

Learning:

* Same table can be joined with itself using Self Join.
* Match start and end records using machine_id and process_id.
* Processing Time = end_timestamp - start_timestamp.
* Use AVG() to find average processing time.
* Use ROUND(..., 3) to show 3 decimal places.

## Problem 11: Employee Bonus

Concepts:

* LEFT JOIN
* IS NULL
* OR

Query:

Select e.name,b.bonus 
from Employee e 
left join Bonus b 
on e.empId=b.empId
where b.Bonus < 1000 
or b.Bonus is null

Learning:

* Use LEFT JOIN to keep employees without bonus records.
* IS NULL helps find missing values.

## Problem 12: Students and Examinations

Concepts:

* CROSS JOIN
* LEFT JOIN
* COUNT()
* GROUP BY
* ORDER BY

Query:

Select s.student_id,s.student_name,sub.subject_name,count(e.student_id) as attended_exams 
from Students s
cross join Subjects sub
left join Examinations e
on s.student_id=e.student_id 
and sub.subject_name=e.subject_name
group by s.student_id,s.student_name,sub.subject_name
order by s.student_id,sub.subject_name asc

Learning:

* CROSS JOIN creates all possible combinations.
* COUNT() counts matching exam records.

## Problem 13: Managers with at Least 5 Direct Reports

Concepts:

* Self Join
* GROUP BY
* HAVING
* COUNT()

Query:

Select e1.name
from Employee e1
join Employee e2
on e1.id=e2.managerId
group by e1.id
having count(*) >= 5;

Learning:

* HAVING filters grouped results.
* Group by unique IDs, not names.
* Self Join can connect managers with employees.

## Problem 14: Confirmation Rate

Concepts:

* LEFT JOIN
* CASE WHEN
* AVG()
* ROUND()

Query:

Select sig.user_id,
round(avg(case when action = 'confirmed' then 1 else 0 end),2) as confirmation_rate
from Signups sig
left join Confirmations con
on sig.user_id = con.user_id
group by sig.user_id

Learning:

* CASE WHEN converts text into numeric values.
* AVG() can calculate confirmation rates directly.
