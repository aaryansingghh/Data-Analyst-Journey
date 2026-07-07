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

## Problem 15: Not Boring Movies

Concepts:

* WHERE
* MODULO (%)
* ORDER BY DESC

Learning:

* Use % to find odd or even numbers.
* id % 2 = 1 returns odd IDs.
* Filter unwanted rows using !=.
* ORDER BY rating DESC sorts highest ratings first.

Query:
Select * from Cinema 
where description != 'boring' and id % 2 = 1
order by rating desc;

## Problem 16: Average Selling Price

Concepts:

* LEFT JOIN
* BETWEEN
* SUM()
* ROUND()
* IFNULL()
* Weighted Average

Learning:

* Use BETWEEN to match purchase dates with the correct price period.
* Average Selling Price = SUM(price × units) / SUM(units).
* LEFT JOIN is needed because products with no sales must still appear.
* IFNULL() converts NULL results to 0.
* ROUND(..., 2) rounds the answer to 2 decimal places.

Query:
Select p.product_id,round(ifnull(sum(p.price * u.units)/sum(u.units),0),2) as average_price 
from Prices p left join UnitsSold u on
p.product_id=u.product_id
and u.purchase_date between p.start_date and p.end_date
group by p.product_id

## Problem 17: Project Employees I

Concepts:

* JOIN
* AVG()
* ROUND()
* GROUP BY

Learning:

* JOIN Project and Employee tables using employee_id.
* AVG() automatically calculates total experience ÷ number of employees.
* GROUP BY project_id calculates the average for each project separately.
* ROUND(...,2) formats the result to 2 decimal places.

Query:
Select p.project_id,round(avg(emp.experience_years),2) as average_years
from Project p 
left join Employee emp on emp.employee_id=p.employee_id 
group by p.project_id

Key Point:
When a question asks for an average value, use AVG() instead of manually dividing by the number of rows.

## Problem 18: Percentage of Users Attended a Contest

• Count registrations per contest using COUNT().
• Get total users using a subquery: (SELECT COUNT(*) FROM Users)
• Calculate percentage: COUNT(user_id) * 100.0 / Total Users
• Round the result to 2 decimal places using ROUND().
• Sort by percentage DESC and contest_id ASC.

Query Logic:

Select contest_id,round(count(user_id) * 100.0 /(select count(*) from Users), 2) as percentage
from Register
group by contest_id
order by percentage desc, contest_id asc

### Learning
→ COUNT() with GROUP BY for aggregation.
→ Subquery inside a mathematical expression.
→ ROUND() for formatting results.

## Problem 19: Queries Quality and Percentage

* Group records by query_name using GROUP BY.
* Calculate query quality as:- AVG(rating / position)
* Identify poor queries where rating < 3.
* Calculate poor query percentage:- (Poor Queries / Total Queries) × 100
* Round both values to 2 decimal places.

### Query Logic

Select query_name,round(avg(cast(rating as decimal) / position), 2) as quality,
round(avg(case when rating < 3 then 1.0 else 0 END) * 100, 2) as poor_query_percentage
from Queries
group by query_name;

### Learning

* AVG() can directly calculate averages of expressions.
* CASE WHEN is useful for conditional counting.
* AVG(0,1 values) can be used to calculate percentages.
* GROUP BY helps generate metrics for each category.

## Problem 20: Monthly Transactions I

Query logic:

Select left(trans_date, 7) as month,
country,count(*) as trans_count,
sum(state = 'approved') as approved_count,
sum(amount) as trans_total_amount,
sum(case when state = 'approved' then amount else 0 END) as approved_total_amount
from Transactions
group by left(trans_date, 7),country;

💡 Approach:

- Extract the **Year-Month (YYYY-MM)** using LEFT(trans_date, 7).
- Group the records by **month** and **country**.
- Use COUNT(*) to count all transactions.
- Use **conditional aggregation** to count only approved transactions.
- Use SUM(amount) to calculate the total transaction amount.
- Use CASE WHEN inside SUM() to calculate the total amount of approved transactions.

 Learnings:

* LEFT(trans_date, 7) extracts the month in YYYY-MM format.
* COUNT(*) counts all rows within each group.
* SUM(state = 'approved') counts approved transactions in **MySQL** because TRUE = 1 and FALSE = 0.
* SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) sums only the approved transaction amounts.
* Conditional aggregation allows multiple statistics to be calculated in a single query.

## Problem 21: Immediate Food Delivery II

Concepts:

* JOIN
* GROUP BY
* MIN()
* CASE WHEN
* COUNT()
* ROUND()

Query:

Select round(sum(case when d.order_date = d.customer_pref_delivery_date then 1 else 0 end ) * 100.0 / COUNT(*),2) 
as immediate_percentage
from Delivery d
join (select customer_id,min(order_date) as first_order from Delivery group by customer_id) f
on d.customer_id = f.customer_id
and d.order_date = f.first_order;

Learning:

* MIN(order_date) finds the first order of each customer.
* JOIN is used to retrieve the complete first-order record.
* CASE WHEN counts only immediate deliveries.
* COUNT(*) gives the total number of first orders.
* ROUND() formats the percentage to 2 decimal places.

## Problem 22: Game Play Analysis IV

Concepts:

* JOIN
* GROUP BY
* MIN()
* DATE_ADD()
* COUNT()
* DISTINCT
* ROUND()

Query:

Select round(count(*) * 1.0/(select count(distinct player_id) from Activity),2) as fraction
from Activity a
join(select player_id,min(event_date) as first_login
from Activity group by player_id) f 
on a.player_id = f.player_id 
and a.event_date = date_add(f.first_login, interval 1 day);

Learning:

* MIN(event_date) finds the first login date of each player.
* JOIN combines the first login data with the original table.
* DATE_ADD() checks whether a player logged in exactly one day after their first login.
* COUNT(*) counts players who logged in the next day.
* COUNT(DISTINCT player_id) counts the total number of unique players.
* ROUND() rounds the final fraction to 2 decimal places.

## Problem 23: Number of Unique Subjects Taught by Each Teacher

Concepts:

*COUNT(DISTINCT)
*GROUP BY

Query:

Select teacher_id,count(distinct subject_id) as cnt from Teacher
group by teacher_id

Learning:

* COUNT(DISTINCT column_name) counts only unique values.
* GROUP BY groups records for each teacher.
* The same subject_id taught in different departments is counted only once.
* COUNT(DISTINCT) is commonly used to remove duplicate values while counting.

## Problem 24: User Activity for the Past 30 Days I

Concepts:

* COUNT(DISTINCT)
* GROUP BY
* WHERE
* BETWEEN

Query:

Select activity_date as day,count(distinct user_id) as active_users from Activity
where activity_date between '2019-06-28' and '2019-07-27'
group by activity_date

Learning:
* COUNT(DISTINCT user_id) counts unique active users for each day.
* BETWEEN filters records within a specific date range.
* GROUP BY activity_date groups the data by each day.
* AS is used to rename the output columns (day and active_users).

## Problem 25: Product Sales Analysis III

Concepts:

* JOIN
* GROUP BY
* MIN()
* Subquery

Query:

Select s.product_id,f.first_year,s.quantity,s.price
from Sales s
join ( Select product_id,min(year) as first_year
from Sales
group by product_id)f
on s.product_id = f.product_id
and s.year = f.first_year;

Learning:

* MIN(year) finds the first year each product was sold.
* GROUP BY product_id groups sales records by product.
* JOIN retrieves the quantity and price for the first sale year.
* Matching both product_id and first_year ensures only the first-year sales records are returned.

## Problem 26: Classes With At Least 5 Students

Concepts:

* GROUP BY
* COUNT()
* HAVING

Query:

Select class
from Courses
group by class
having count(student) >= 5;

Learning:

* GROUP BY groups all students by class.
* COUNT(student) counts the number of students in each class.
* HAVING filters grouped results based on aggregate functions.
* Use HAVING instead of WHERE when filtering with COUNT(), SUM(), AVG(), etc.

## Problem 27: Find Followers Count

Concepts:-

* COUNT()
* GROUP BY
* ORDER BY

Query:

Select user_id,count(follower_id) as followers_count
from Followers
group by user_id
order by user_id;

Learning:

* COUNT(follower_id) counts the total followers for each user.
* GROUP BY user_id groups all followers of the same user.
* ORDER BY user_id sorts the result in ascending order.
* DISTINCT is not required because (user_id, follower_id) is already a unique primary key.

## Problem 28: Biggest Single Number

Concepts:

* MAX()
* GROUP BY
* HAVING
* COUNT()
* Subquery

Query:

Select max(num) as num from MyNumbers
where num in (
Select num from MyNumbers 
group by num having count(num)=1)

Learning:

* GROUP BY num groups identical numbers together.
* HAVING COUNT(num) = 1 filters numbers that appear only once.
* WHERE num IN (...) keeps only the single numbers.
* MAX(num) returns the largest single number.
* MAX() returns NULL automatically if no single number exists.

## Problem 29: Customers Who Bought All Products

Concepts:

* COUNT(DISTINCT)
* GROUP BY
* HAVING
* Subquery

Query:

Select customer_id from Customer
group by customer_id
having count(distinct product_key) = (select count(*) from product)

Learning:

* COUNT(DISTINCT product_key) counts the unique products purchased by each customer.
* GROUP BY customer_id groups all purchases by customer.
* The subquery COUNT(*) calculates the total number of products.
* HAVING compares the number of products bought by each customer with the total number of available products.
* If both counts are equal, the customer has purchased all products.

## Problem 30: The Number of Employees Which Report to Each Employee

Concepts:

* Self JOIN
* COUNT()
* AVG()
* ROUND()
* GROUP BY
* ORDER BY

Query:

Select m.employee_id,m.name,
count(e.employee_id) as reports_count,
round(avg(e.age), 0) as average_age
from Employees e
join Employees m
on e.reports_to = m.employee_id
group by m.employee_id,m.name
order by m.employee_id;

Learning:
* Self JOIN is used because managers and employees are stored in the same table.
* COUNT(e.employee_id) counts the number of employees reporting to each manager.
* AVG(e.age) calculates the average age of the reporting employees.
* ROUND() rounds the average age to the nearest integer.
* GROUP BY groups all employees under their respective manager.
* ORDER BY employee_id sorts the output in ascending order.

## Problem 31: Primary Department for Each Employee

Concepts:

* UNION
* GROUP BY
* HAVING
* COUNT()
* WHERE

Query:

Select distinct employee_id,department_id from Employee 
where primary_flag = 'Y'
union Select distinct employee_id,department_id from Employee 
group by employee_id
having count(*) = 1;

Learning:

* primary_flag = 'Y' identifies the primary department for employees in multiple departments.
* COUNT(*) = 1 finds employees who belong to only one department.
* UNION combines both result sets and removes duplicates.
* GROUP BY employee_id groups records for each employee.
* HAVING filters grouped results based on the number of departments.

## Problem 32: Triangle Judgement

A triangle can be formed only if:
* x + y > z
* x + z > y
* y + z > x
If all three conditions are true → Yes
Otherwise → No

Concepts:

* CASE WHEN
* Logical Operators (AND)

Query:

Select x,y,z,
case when x + y > z
and x + z > y
and y + z > x
then 'Yes'
else 'No' end as  triangle
from Triangle;

Learning:

* A triangle is valid only if the sum of any two sides is greater than the third side.
* CASE WHEN is used to apply conditional logic in SQL.
* AND ensures that all three triangle conditions must be true.
* ELSE returns 'No' if any one of the conditions fails.

## Problem 33: Consecutive Numbers

Concepts:
* Self JOIN
* Multiple Table Aliases
* DISTINCT
* WHERE
* AND

Query:

Select distinct l1.num as ConsecutiveNums
from Logs l1 join Logs l2
on l1.id = l2.id - 1 join Logs l3
on l2.id = l3.id - 1 where l1.num = l2.num
and l2.num = l3.num;

Learning:

* Self JOIN is used to compare rows within the same table.
* Three aliases (l1, l2, l3) represent three consecutive rows.
* l1.id = l2.id - 1 and l2.id = l3.id - 1 ensure the rows are consecutive.
* WHERE l1.num = l2.num AND l2.num = l3.num checks if the same number appears three times consecutively.
* DISTINCT removes duplicate results if the same number appears in multiple consecutive sequences.

## Problem 34: Product Price at a Given Date

Concepts:

* MAX()
* MIN()
* GROUP BY
* HAVING
* WHERE
* UNION
* Tuple Comparison ((col1, col2) IN (...))

Query:

Select product_id,new_price as price
from Products
where (product_id, change_date) in (Select product_id,max(change_date)
from Products where change_date <= '2019-08-16'
group by product_id)
union Select product_id,10 as price
from Products
group by product_id
having min(change_date) > '2019-08-16';

Learning:

* MAX(change_date) finds the latest price update before or on the given date.
* MIN(change_date) identifies products whose first update occurred after the given date.
* UNION combines updated prices with products that still have the default price.
* HAVING filters grouped results based on aggregate values.
* Products without any price update before 2019-08-16 keep the default price of 10.

## Problem 35: Last Person to Fit in the Bus

Concepts:

* Window Function
* SUM() OVER()
* ORDER BY
* Subquery
* LIMIT

Query:
Select person_name
from (Select person_name,sum(weight) over (order by turn) as total_weight
from Queue) t
where total_weight <= 1000
order by total_weight desc
limit 1;

Learning:

* SUM() OVER(ORDER BY turn) calculates the cumulative weight in boarding order.
* The window function keeps a running total without grouping the rows.
* WHERE total_weight <= 1000 filters people who can still board.
* ORDER BY total_weight DESC LIMIT 1 returns the last person who fits within the weight limit.

## Problem 36: Count Salary Categories

Concepts:
* CASE WHEN
* COUNT()
* UNION ALL
* Conditional Aggregation

Query:

Select 'Low Salary' as category,
count(case when income < 20000 then 1 end) as accounts_count 
from Accounts
union all Select 'Average Salary',
count(case when income between 20000 and 50000 then 1 end)
from Accounts
union all Select 'High Salary',
count(case when income > 50000 then 1 end)
from Accounts;

Learning:

* CASE WHEN classifies each income into a salary category.
* COUNT(CASE WHEN ... THEN 1 END) counts only rows that satisfy the condition.
* UNION ALL combines the three salary categories into one result.
* UNION ALL is used instead of UNION because all three rows must always be returned, including categories with 0 accounts.

## Problem 37: Employees Whose Manager Left the Company

Concepts:
* NOT IN
* Subquery
* WHERE
* ORDER BY

Query:

Select employee_id from Employees
where salary < 30000
and manager_id is not null
and manager_id not in (Select employee_id from Employees)
order by employee_id

Learning:

* salary < 30000 filters employees with salaries below 30000.
* manager_id IS NOT NULL excludes employees who don't have a manager.
* NOT IN with a subquery finds managers whose IDs no longer exist in the table.
* ORDER BY employee_id returns the result in ascending order.

## Problem 38: Exchange Seats

Concepts:
* CASE WHEN
* MOD (%)
* Subquery
* MAX()
* ORDER BY

Query:

Select case
when id % 2 = 1 and id != (Select max(id) from Seat) then id + 1
when id % 2 = 0 then id - 1
else id
end as id,
student
from Seat
order by id;

Learning:

* id % 2 = 1 identifies odd seat IDs.
* id % 2 = 0 identifies even seat IDs.
* MAX(id) finds the last seat ID.
* CASE WHEN swaps consecutive seat IDs.
* ORDER BY id displays the final seating arrangement in ascending order.

