# Leetcode_SQL_50
### 1. Recyclable and Low Fat Products (Easy) 1757.

Table: Products
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_id  | int     |
| low_fats    | enum    |
| recyclable  | enum    |
+-------------+---------+
product_id is the primary key (column with unique values) for this table.
low_fats is an ENUM (category) of type ('Y', 'N') where 'Y' means this product is low fat and 'N' means it is not.
recyclable is an ENUM (category) of types ('Y', 'N') where 'Y' means this product is recyclable and 'N' means it is not.
```
Write a solution to find the ids of products that are both low fat and recyclable.
Return the result table in any order.

```SQL
SELECT product_id
FROM Products
WHERE low_fats = 'Y' and recyclable = 'Y';
```
### 2. Find Customer Referee (Easy) 584.

Table: Customer
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| referee_id  | int     |
+-------------+---------+
In SQL, id is the primary key column for this table.
Each row of this table indicates the id of a customer, their name, and the id of the customer who referred them.
```
Find the names of the customer that are not referred by the customer with id = 2.
Return the result table in any order.
The result format is in the following example.

```SQL
SELECT name
FROM Customer
WHERE referee_id is null or referee_id !=2;
```
### 3. Big Countries (Easy) 595.

Table: World
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |
+-------------+---------+
name is the primary key (column with unique values) for this table.
Each row of this table gives information about the name of a country, the continent to which it belongs, its area, the population, and its GDP value.
```
A country is big if:
it has an area of at least three million (i.e., 3000000 km2), or
it has a population of at least twenty-five million (i.e., 25000000).
Write a solution to find the name, population, and area of the big countries.
Return the result table in any order.
```SQL
SELECT name, population, area
FROM WORLD
WHERE population >= 25000000 OR area >= 3000000;
```
### 4. Article Views I (easy) 1148.

Table: Views
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| article_id    | int     |
| author_id     | int     |
| viewer_id     | int     |
| view_date     | date    |
+---------------+---------+
There is no primary key (column with unique values) for this table, the table may have duplicate rows.
Each row of this table indicates that some viewer viewed an article (written by some author) on some date. 
Note that equal author_id and viewer_id indicate the same person.
```
Write a solution to find all the authors that viewed at least one of their own articles.
Return the result table sorted by id in ascending order.

```SQL
SELECT DISTINCT author_id AS id 
FROM Views
WHERE author_id = viewer_id
ORDER BY author_id;
```
### 5. Invalid Tweets (easy) 1663. 

Table: Tweets
```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| tweet_id       | int     |
| content        | varchar |
+----------------+---------+
tweet_id is the primary key (column with unique values) for this table.
content consists of characters on an American Keyboard, and no other special characters.
This table contains all the tweets in a social media app.
```
Write a solution to find the IDs of the invalid tweets. The tweet is invalid if the number 
of characters used in the content of the tweet is strictly greater than 15.
Return the result table in any order.

```SQL
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15;
```
### 6. Replace Employee ID With the Unique Identifier (Easy) 1378.

Table: Employees
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table contains the id and the name of an employee in a company.
 
Table: EmployeeUNI
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
(id, unique_id) is the primary key (combination of columns with unique values) for this table.
Each row of this table contains the id and the corresponding unique id of an employee in the company.
```
Write a solution to show the unique ID of each user, If a user does not have a unique ID replace just show null.
Return the result table in any order.

```SQL
SELECT p.unique_id, c.name
FROM Employees c
LEFT JOIN EmployeeUNI AS p ON c.id = p.id;
```
### 7. Product Sales Analysis I (easy) 1068.

Table: Sales
```
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+
(sale_id, year) is the primary key (combination of columns with unique values) of this table.
product_id is a foreign key (reference column) to Product table.
Each row of this table shows a sale on the product product_id in a certain year.
Note that the price is per unit.
 
Table: Product
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
product_id is the primary key (column with unique values) of this table.
Each row of this table indicates the product name of each product.
```
Write a solution to report the product_name, year, and price for each sale_id in the Sales table.
Return the resulting table in any order.
```SQL
SELECT p.product_name, s.year, s.price
FROM Sales AS s
JOIN Product AS p ON p.product_id = s.product_id
```
### 8. Customer Who Visited but did not Make any Transactions

Table: Visits
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| visit_id    | int     |
| customer_id | int     |
+-------------+---------+
visit_id is the column with unique values for this table.
This table contains information about the customers who visited the mall.
 
Table: Transactions
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |
+----------------+---------+
transaction_id is column with unique values for this table.
This table contains information about the transactions made during the visit_id.
```
Write a solution to find the IDs of the users who visited without making 
any transactions and the number of times they made these types of visits.
Return the result table sorted in any order.

```SQL
SELECT v.customer_id, COUNT(v.visit_id) AS count_no_trans 
FROM Visits AS v
LEFT JOIN transactions t ON v.visit_id = t.visit_id
WHERE t.transaction_id IS NULL
GROUP BY v.customer_id
```
### 9. Rising Temperature (Easy) 197.

Table: Weather
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id is the column with unique values for this table.
There are no different rows with the same recordDate.
This table contains information about the temperature on a certain day.
```
Write a solution to find all dates' id with higher temperatures compared to its previous dates (yesterday).
Return the result table in any order.
```SQL
SELECT w2.id 
FROM weather w1, weather w2
WHERE datediff(w2.recordDate, w1.recordDate) = 1 
AND w2.temperature > w1.temperature;
```
### 10. Average Time of Process pre Machine (easy) 1661.

Table: Activity
```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+
The table shows the user activities for a factory website.
(machine_id, process_id, activity_type) is the primary key (combination of columns with unique values) of this table.
machine_id is the ID of a machine.
process_id is the ID of a process running on the machine with ID machine_id.
activity_type is an ENUM (category) of type ('start', 'end').
timestamp is a float representing the current time in seconds.
'start' means the machine starts the process at the given timestamp and 'end' means the machine ends the process at the given timestamp.
The 'start' timestamp will always be before the 'end' timestamp for every (machine_id, process_id) pair.
It is guaranteed that each (machine_id, process_id) pair has a 'start' and 'end' timestamp.
```
There is a factory website that has several machines each running the same number of processes.
Write a solution to find the average time each machine takes to complete a process.
The time to complete a process is the 'end' timestamp minus the 'start' timestamp. 
The average time is calculated by the total time to complete every process on 
the machine divided by the number of processes that were run.
The resulting table should have the machine_id along with the 
average time as processing_time, which should be rounded to 3 decimal places.
Return the result table in any order.

```SQL
SELECT a.machine_id, 
    ROUND(AVG(b.timestamp - a.timestamp), 3) AS processing_time
FROM Activity a
JOIN Activity b
ON a.machine_id = b.machine_id
AND a.activity_type != b.activity_type
WHERE a.activity_type = 'start'
GROUP BY a.machine_id;
-----------------------------------------------------------------
SELECT
    a.machine_id,
    ROUND(AVG(b.timestamp - a.timestamp), 3) AS processing_time
FROM Activity a
JOIN Activity b
ON a.machine_id = b.machine_id
    AND a.activity_type = 'start'
    AND b.activity_type = 'end'
GROUP BY a.machine_id;
```
### 11. Employees Bonus (easy) 577.

Table: Employee
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
+-------------+---------+
empId is the column with unique values for this table.
Each row of this table indicates the name and the ID of an employee in addition to their salary and the id of their manager.

Table: Bonus
+-------------+------+
| Column Name | Type |
+-------------+------+
| empId       | int  |
| bonus       | int  |
+-------------+------+
empId is the column of unique values for this table.
empId is a foreign key (reference column) to empId from the Employee table.
Each row of this table contains the id of an employee and their respective bonus.
```
Write a solution to report the name and bonus amount of each employee with a bonus less than 1000.
Return the result table in any order.

```SQL
SELECT e.name, b.bonus
FROM Employee AS e
LEFT JOIN Bonus AS b 
ON e.empId = b.empId
WHERE b.bonus < 1000 OR b.bonus IS NULL
```
### 12. Students and Examinations (easy) 1280.

Table: Srudents
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
student_id is the primary key (column with unique values) for this table.
Each row of this table contains the ID and the name of one student in the school.

Table: Subjects
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| subject_name | varchar |
+--------------+---------+
subject_name is the primary key (column with unique values) for this table.
Each row of this table contains the name of one subject in the school.
 
Table: Examinations
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| subject_name | varchar |
+--------------+---------+
There is no primary key (column with unique values) for this table. It may contain duplicates.
Each student from the Students table takes every course from the Subjects table.
Each row of this table indicates that a student with ID student_id attended the exam of subject_name.

```
Write a solution to find the number of times each student attended each exam.
Return the result table ordered by student_id and subject_name.
```SQL
SELECT s.student_id, s.student_name, c.subject_name,
    COUNT(e.subject_name) AS attended_exams
FROM Students s
JOIN Subjects c
LEFT JOIN Examinations e
ON s.student_id = e.student_id 
AND e.subject_name = c.subject_name
GROUP BY s.student_id, c.subject_name
ORDER BY student_id, subject_name
```
### 13. Managers with at Least 5 Direct Reports (easy) 570.

Table: Employee
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table indicates the name of an employee, their department, and the id of their manager.
If managerId is null, then the employee does not have a manager.
No employee will be the manager of themself.
```
Write a solution to find managers with at least five direct reports.
Return the result table in any order.

```SQL
SELECT e.name
FROM Employee e
JOIN Employee e1
ON e.id = e1.managerId
GROUP BY e.id
HAVING COUNT(*) >= 5
```
### 14. Confirmation Rate (Medium) 1934.

Table: Signups
```
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
+----------------+----------+
user_id is the column of unique values for this table.
Each row contains information about the signup time for the user with ID user_id.
 
Table: Confirmations
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
| action         | ENUM     |
+----------------+----------+
(user_id, time_stamp) is the primary key (combination of columns with unique values) for this table.
user_id is a foreign key (reference column) to the Signups table.
action is an ENUM (category) of the type ('confirmed', 'timeout')
Each row of this table indicates that the user with ID user_id requested a confirmation message at time_stamp and that confirmation message was either confirmed ('confirmed') or expired without confirming ('timeout').

```
The confirmation rate of a user is the number of 'confirmed' messages divided by the total 
number of requested confirmation messages. The confirmation rate of a user that did not 
request any confirmation messages is 0. Round the confirmation rate to two decimal places.
Write a solution to find the confirmation rate of each user.
Return the result table in any order.

```SQL
SELECT 
    s.user_id,
    ROUND(AVG(IF(action = 'confirmed', 1, 0)), 2) AS confirmation_rate
FROM Signups AS s
LEFT JOIN Confirmations AS c
ON s.user_id = c.user_id
GROUP BY s.user_id
---------------------------------------------------------------------
SELECT 
    s.user_id,
    ROUND(
        CASE 
            WHEN c.user_id IS NULL THEN 0
            ELSE SUM(c.action = "confirmed")/COUNT(c.action)
        END, 2)
        AS confirmation_rate
FROM Signups s
LEFT JOIN Confirmations c
ON s.user_id = c.user_id
GROUP BY user_id
```
### 15. Not Boring Movies (Easy) 620.

Table: Cinema
```
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| id             | int      |
| movie          | varchar  |
| description    | varchar  |
| rating         | float    |
+----------------+----------+
id is the primary key (column with unique values) for this table.
Each row contains information about the name of a movie, its genre, and its rating.
rating is a 2 decimal places float in the range [0, 10]
```
Write a solution to report the movies with an odd-numbered ID and a description that is not "boring".
Return the result table ordered by rating in descending order.
```SQL
SELECT id, movie, description, rating
FROM Cinema
WHERE id % 2 != 0 
AND description NOT LIKE '%boring%' 
ORDER BY rating DESC
```
### 16. Average Selling Price (Easy) 1251.

Table: Prices
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| start_date    | date    |
| end_date      | date    |
| price         | int     |
+---------------+---------+
(product_id, start_date, end_date) is the primary key (combination of columns with unique values) for this table.
Each row of this table indicates the price of the product_id in the period from start_date to end_date.
For each product_id there will be no two overlapping periods. That means there will be no two intersecting periods for the same product_id.
 
Table: UnitsSold
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| purchase_date | date    |
| units         | int     |
+---------------+---------+
This table may contain duplicate rows.
Each row of this table indicates the date, units, and product_id of each product sold.
```
Write a solution to find the average selling price for each product. average_price 
should be rounded to 2 decimal places. If a product does not have any sold units, 
its average selling price is assumed to be 0.
Return the result table in any order.
```SQL
SELECT p.product_id,
    ROUND(
        CASE 
            WHEN u.units IS NULL THEN 0
            ELSE SUM(p.price * u.units) / SUM(u.units) 
        END, 
    2) AS average_price
FROM Prices AS p
LEFT JOIN UnitsSold AS u
ON p.product_id = u.product_id
WHERE u.purchase_date >= p.start_date AND u.purchase_date <= p.end_date OR u.purchase_date IS NULL
GROUP BY p.product_id
-------------------------------------------------------------------------------------------------
SELECT 
    p.product_id, 
    COALESCE(ROUND(SUM(p.price * u.units) / SUM(u.units), 2), 0) AS average_price
FROM Prices AS p
LEFT JOIN UnitsSold u
ON p.product_id = u.product_id
AND u.purchase_date BETWEEN p.start_date and p.end_date
GROUP BY p.product_id 
```
### 17. Project Employees I (Easy) 1075.

Table: Project
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| project_id  | int     |
| employee_id | int     |
+-------------+---------+
(project_id, employee_id) is the primary key of this table.
employee_id is a foreign key to Employee table.
Each row of this table indicates that the employee with employee_id is working on the project with project_id.
 
Table: Employee
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |
+------------------+---------+
employee_id is the primary key of this table. It's guaranteed that experience_years is not NULL.
Each row of this table contains information about one employee.
```
Write an SQL query that reports the average experience years of all the employees for each project, rounded to 2 digits.
Return the result table in any order.
```SQL
SELECT p.project_id, 
    ROUND(SUM(e.experience_years)/count(e.name), 2) AS average_years
FROM Project AS p
LEFT JOIN Employee AS e
ON p.employee_id = e.employee_id
GROUP BY project_id
```
### 18. Percentage of Users Attended a Contest (Easy) 1633.

Table: Users
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| user_name   | varchar |
+-------------+---------+
user_id is the primary key (column with unique values) for this table.
Each row of this table contains the name and the id of a user.
 
Table: Register
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| contest_id  | int     |
| user_id     | int     |
+-------------+---------+
(contest_id, user_id) is the primary key (combination of columns with unique values) for this table.
Each row of this table contains the id of a user and the contest they registered into.
```
Write a solution to find the percentage of the users registered in each contest rounded to two decimals.
Return the result table ordered by percentage in descending order. 
In case of a tie, order it by contest_id in ascending order.
```SQL
SELECT r.contest_id, 
    ROUND(COUNT(r.user_id) * 100/(SELECT COUNT(DISTINCT u.user_id) FROM Users u), 2) AS percentage
FROM Register AS r
JOIN Users AS u 
ON u.user_id = r.user_id
GROUP BY r.contest_id
ORDER BY percentage DESC, contest_id
```
### 19.  (Easy) .

Table: 
```

```

```SQL

```
### 20.  (Easy) .

Table: 
```

```

```SQL

```
### 21.  (Easy) .

Table: 
```

```

```SQL

```
### 22.  (Easy) .

Table: 
```

```

```SQL

```
### 23.  (Easy) .

Table: 
```

```

```SQL

```
### 24.  (Easy) .

Table: 
```

```

```SQL

```
### 25.  (Easy) .

Table: 
```

```

```SQL

```
### 26.  (Easy) .

Table: 
```

```

```SQL

```
### 27.  (Easy) .

Table: 
```

```

```SQL

```
### 28.  (Easy) .

Table: 
```

```

```SQL

```
### 29.  (Easy) .

Table: 
```

```

```SQL

```
30. Triangle Judgement
      Table: Triangle
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| x           | int  |
| y           | int  |
| z           | int  |
+-------------+------+
```
In SQL, (x, y, z) is the primary key column for this table.
Each row of this table contains the lengths of three line segments.
Report for every three line segments whether they can form a triangle.
Return the result table in any order.

```SQL

SELECT x, y, z,
  CASE 
    WHEN (x+y > z AND y+z > x AND z+x > y) THEN 'Yes'
    ELSE 'No'
  END AS triangle
FROM Triangle

```

  31. Consecutive Numbers
      Table: Logs
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
In SQL, id is the primary key for this table.
id is an autoincrement column starting from 1.
```
Find all numbers that appear at least three times consecutively.
Return the result table in any order.

```SQL

SELECT DISTINCT l1.num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2 ON l1.id = l2.id - 1
JOIN Logs l3 ON l2.id = l3.id - 1
WHERE l1.num = L2.num AND l2.num = l3.num

```

  32. Product Price at a Given Date
      Table: Products
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |
+---------------+---------+
(product_id, change_date) is the primary key (combination of columns with unique values) of this table.
Each row of this table indicates that the price of some product was changed to a new price at some date.
```
Write a solution to find the prices of all products on 2019-08-16. Assume the price of all products before any change is 10.
Return the result table in any order.

```SQL

SELECT 
    product_id, 
    COALESCE(
        (SElECT new_price
        FROM Products p2
        WHERE p2.product_id = p1.product_id
        AND p2.change_date <= '2019-08-16'
        ORDER BY p2.change_date DESC
        LIMIT 1), 10) AS price
        
FROM (SELECT DISTINCT product_id FROM Products) p1;

```

 33. Last Person to fit in the Bus
      Table: Queue
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| person_id   | int     |
| person_name | varchar |
| weight      | int     |
| turn        | int     |
+-------------+---------+
person_id column contains unique values.
This table has the information about all people waiting for a bus.
The person_id and turn columns will contain all numbers from 1 to n, where n is the number of rows in the table.
turn determines the order of which the people will board the bus, where turn=1 denotes the first person to board and turn=n denotes the last person to board.
weight is the weight of the person in kilograms.
```
There is a queue of people waiting to board a bus. However, 
the bus has a weight limit of 1000 kilograms, so there may be some people who cannot board.
Write a solution to find the person_name of the last person that can
fit on the bus without exceeding the weight limit. 
The test cases are generated such that the first person does not exceed the weight limit.
Note that only one person can board the bus at any given turn.

```SQL

SELECT t1.person_name
FROM Queue t1
JOIN Queue t2 ON t1.turn >= t2.turn
GROUP BY t1.turn
HAVING SUM(t2.weight) <= 1000
ORDER BY SUM(t2.weight) DESC
LIMIT 1

```
 34. Last Person to fit in the Bus
      Table: Queue
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| person_id   | int     |
| person_name | varchar |
| weight      | int     |
| turn        | int     |
+-------------+---------+
person_id column contains unique values.
This table has the information about all people waiting for a bus.
The person_id and turn columns will contain all numbers from 1 to n, where n is the number of rows in the table.
turn determines the order of which the people will board the bus, where turn=1 denotes the first person to board and turn=n denotes the last person to board.
weight is the weight of the person in kilograms.
```
There is a queue of people waiting to board a bus. However, 
the bus has a weight limit of 1000 kilograms, so there may be some people who cannot board.
Write a solution to find the person_name of the last person that can
fit on the bus without exceeding the weight limit. 
The test cases are generated such that the first person does not exceed the weight limit.
Note that only one person can board the bus at any given turn.

```SQL
# Very slow variant using JOIN

SELECT t1.person_name
FROM Queue t1
JOIN Queue t2 ON t1.turn >= t2.turn
GROUP BY t1.turn
HAVING SUM(t2.weight) <= 1000
ORDER BY SUM(t2.weight) DESC
LIMIT 1;

```
Or

```SQL
# Very good perfomance

SELECT person_name 
FROM (SELECT person_name,
             turn,
             sum(weight)
     OVER (ORDER BY turn) AS cum 
     FROM queue) p1
WHERE cum <= 1000 
ORDER BY turn DESC 
LIMIT 1;

```

35. Count Salary Categories
      Table: Accounts
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| account_id  | int  |
| income      | int  |
+-------------+------+
account_id is the primary key (column with unique values) for this table.
Each row contains information about the monthly income for one bank account.
```
Write a solution to calculate the number of bank accounts for each salary category. The salary categories are:

"Low Salary": All the salaries strictly less than $20000.
"Average Salary": All the salaries in the inclusive range [$20000, $50000].
"High Salary": All the salaries strictly greater than $50000.
The result table must contain all three categories. If there are no accounts in a category, return 0.

Return the result table in any order.

```SQL

SELECT 
    'Low Salary' AS category, 
    COUNT(*) AS accounts_count 
FROM Accounts 
WHERE income < 20000
UNION
SELECT 
    'Average Salary' AS category, 
    COUNT(*) AS accounts_count 
FROM accounts 
WHERE income 
BETWEEN 20000 AND 50000
UNION
SELECT 
    'High Salary' AS category, 
    COUNT(*) AS accounts_count 
FROM accounts 
WHERE income > 50000 ;


```
36. Employees Whose Manager Left the Company
      Table: Employees
```
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| employee_id | int      |
| name        | varchar  |
| manager_id  | int      |
| salary      | int      |
+-------------+----------+
In SQL, employee_id is the primary key for this table.
This table contains information about the employees, their salary, and the ID of their manager. Some employees do not have a manager (manager_id is null). 
```
Find the IDs of the employees whose salary is strictly 
less than $30000 and whose manager left the company.
When a manager leaves the company, their
information is deleted from the Employees table, 
but the reports still have their manager_id set to the manager that left.
Return the result table ordered by employee_id.

```SQL
-- Join

SELECT e.employee_id
FROM employees e
LEFT JOIN employees m 
ON e.manager_id IS NOT NULL 
AND e.manager_id = m.employee_id
WHERE e.salary < 30000
  AND e.manager_id IS NOT NULL
  AND m.employee_id IS NULL
ORDER BY e.employee_id;

```
or 

```SQL
-- Subquery

SELECT employee_id
FROM employees
WHERE salary < 30000
    AND manager_id NOT IN (
        SELECT employee_id
        FROM employees
    )
ORDER BY employee_id;


```
37. Exchange Seats
      Table: Seat
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| student     | varchar |
+-------------+---------+
id is the primary key (unique value) column for this table.
Each row of this table indicates the name and the ID of a student.
The ID sequence always starts from 1 and increments continuously.

```
Write a solution to swap the seat id of every two consecutive students. 
If the number of students is odd, the id of the last student is not swapped.
Return the result table ordered by id in ascending order.

```SQL
SELECT 
    CASE 
        WHEN MOD(id, 2) = 1 AND id + 1 <= (SELECT MAX(id) FROM Seat) THEN id + 1
        WHEN MOD(id, 2) = 0 THEN id - 1
        ELSE id
    END AS id, student
FROM Seat
ORDER BY id;

```
38. Movie Rating
      Table: Movies
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| title         | varchar |
+---------------+---------+
movie_id is the primary key (column with unique values) for this table.
title is the name of the movie.

```

 Table: Users
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| name          | varchar |
+---------------+---------+
user_id is the primary key (column with unique values) for this table.
The column 'name' has unique values.

```

Table: MovieRating
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| user_id       | int     |
| rating        | int     |
| created_at    | date    |
+---------------+---------+
(movie_id, user_id) is the primary key (column with unique values) for this table.
This table contains the rating of a movie by a user in their review.
created_at is the user's review date. 

```

Write a solution to:

Find the name of the user who has rated the greatest number of movies. 
In case of a tie, return the lexicographically smaller user name.
Find the movie name with the highest average rating in February 2020. 
In case of a tie, return the lexicographically smaller movie name.

```SQL
(SELECT name AS results
FROM Users JOIN MovieRating USING(user_id)
GROUP BY name
ORDER BY COUNT(*) DESC, name
LIMIT 1)

UNION ALL

(SELECT title AS results
FROM Movies JOIN MovieRating USING(movie_id)
WHERE created_at BETWEEN '2020-02-01' AND '2020-02-29'
GROUP BY title
ORDER BY AVG(rating) DESC, title
LIMIT 1);

```
39. Restaurant Growth
      Table: Customer
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
| visited_on    | date    |
| amount        | int     |
+---------------+---------+
In SQL,(customer_id, visited_on) is the primary key for this table.
This table contains data about customer transactions in a restaurant.
visited_on is the date on which the customer with ID (customer_id) has visited the restaurant.
amount is the total paid by a customer.

```
You are the restaurant owner and you want to analyze a possible expansion 
(there will be at least one customer every day).
Compute the moving average of how much the customer paid in a seven days window
(i.e., current day + 6 days before). average_amount should be rounded to two decimal places.
Return the result table ordered by visited_on in ascending order.

```SQL
SELECT visited_on, 
    (
        SELECT SUM(amount)
        FROM customer
        WHERE visited_on BETWEEN DATE_SUB(c.visited_on, INTERVAL 6 DAY) AND c.visited_on
    ) AS amount, 
    ROUND(
        (
            SELECT SUM(amount) / 7
            FROM customer
            WHERE visited_on BETWEEN DATE_SUB(c.visited_on, INTERVAL 6 DAY) AND c.visited_on
        ), 2
    ) AS average_amount
FROM customer c
WHERE visited_on >= (
    SELECT DATE_ADD(MIN(visited_on), INTERVAL 6 DAY)
    FROM customer
)
GROUP BY visited_on


```
40. Friend Requests II: Who Has the Most Friends
     Table: RequestAccepted
```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| requester_id   | int     |
| accepter_id    | int     |
| accept_date    | date    |
+----------------+---------+
(requester_id, accepter_id) is the primary key (combination of columns with unique values) for this table.
This table contains the ID of the user who sent the request, the ID of the user who received the request, and the date when the request was accepted.

```

Write a solution to find the people who have the most friends and the most friends number.
The test cases are generated so that only one person has the most friends.

```SQL

WITH base AS (
    SELECT requester_id AS id
    FROM RequestAccepted
    UNION ALL
    SELECT accepter_id 
    FROM RequestAccepted
    )
SELECT id,
    COUNT(*) num 
FROM base
GROUP BY id
ORDER BY COUNT(*) DESC
LIMIT 1

```

41. Investments in 2016
     Table: Insurance
```
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| pid         | int   |
| tiv_2015    | float |
| tiv_2016    | float |
| lat         | float |
| lon         | float |
+-------------+-------+
pid is the primary key (column with unique values) for this table.
Each row of this table contains information about one policy where:
pid is the policyholder's policy ID.
tiv_2015 is the total investment value in 2015 and tiv_2016 is the total investment value in 2016.
lat is the latitude of the policy holder's city. It's guaranteed that lat is not NULL.
lon is the longitude of the policy holder's city. It's guaranteed that lon is not NULL.

```
Write a solution to report the sum of all total investment 
values in 2016 tiv_2016, for all policyholders who:
have the same tiv_2015 value as one or more other policyholders, and
are not located in the same city as any other policyholder 
(i.e., the (lat, lon) attribute pairs must be unique).
Round tiv_2016 to two decimal places.

```SQL

SELECT 
    ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM Insurance
WHERE tiv_2015 IN (
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
) AND (lat, lon) IN (
    SELECT lat, 
           lon
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
)

```
42. Investments in 2016
     Table: Employee
```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
+--------------+---------+
id is the primary key (column with unique values) for this table.
departmentId is a foreign key (reference column) of the ID from the Department table.
Each row of this table indicates the ID, name, and salary of an employee. It also contains the ID of their department.
```
 Table: Department
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table indicates the ID of a department and its name.

```
A company's executives are interested in seeing who earns the most money in each of the company's departments.
A high earner in a department is an employee who has a salary in the top three unique salaries for that department.
Write a solution to find the employees who are high earners in each of the departments.
Return the result table in any order.

```SQL
-- O(n^2) poor perfomance
SELECT 
    d.name AS Department,
    e.name AS Employee,
    e.salary AS Salary
FROM Employee AS e
JOIN Department AS d ON e.departmentId = d.id
WHERE 3 > (
    SELECT COUNT(DISTINCT salary)
    FROM Employee e2
    WHERE e2.salary > e.salary AND e.departmentId = e2.departmentId
);
```

43.  Fix Names in a Table (Easy) 1667.

Table Users
```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| user_id        | int     |
| name           | varchar |
+----------------+---------+
user_id is the primary key (column with unique values) for this table.
This table contains the ID and the name of the user. The name consists of only lowercase and uppercase characters.
```
Write a solution to fix the names so that only the first character is uppercase and the rest are lowercase.
Return the result table ordered by user_id.

```SQL
SELECT 
    user_id,
    CONCAT(UPPER(SUBSTRING(name, 1,1)), LOWER(SUBSTRING(name, 2))) as name
FROM users
ORDER BY user_id;
```
44. Patients with a condition (Easy) 1527.

Table Patients
```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| patient_id   | int     |
| patient_name | varchar |
| conditions   | varchar |
+--------------+---------+
patient_id is the primary key (column with unique values) for this table.
'conditions' contains 0 or more code separated by spaces. 
This table contains information of the patients in the hospital.
```
Write a solution to find the patient_id, patient_name, and conditions of the 
patients who have Type I Diabetes. Type I Diabetes always starts with DIAB1 prefix.
Return the result table in any order.
```SQL
SELECT patient_id, patient_name, conditions
FROM Patients
-- WHERE conditions REGEXP '(^| )DIAB1'
WHERE conditions LIKE 'DIAB1%' or conditions LIKE '% DIAB1%'
```

45. Delete Duplicate Emails (Easy) 196.

Table Person
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table contains an email. The emails will not contain uppercase letters.
```
Write a solution to delete all duplicate emails, keeping only one unique email with the smallest id.
For SQL users, please note that you are supposed to write a DELETE statement and not a SELECT one.
For Pandas users, please note that you are supposed to modify Person in place.
After running your script, the answer shown is the Person table. The driver will first 
compile and run your piece of code and then show the Person table. 
The final order of the Person table does not matter.

```SQL
DELETE p1 
FROM person p1, person p2
WHERE p1.email = p2.email AND p1.id > p2.id;
```
46. Second Highest Salary (Medium) 176.

Table Employee
```
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
id is the primary key (column with unique values) for this table.
Each row of this table contains information about the salary of an employee.
```
Write a solution to find the second highest distinct salary from the Employee table. 
If there is no second highest salary, return null (return None in Pandas).

```SQL
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```
47. Group Sold Products By the Date (Easy) 1484.

Table Activities
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| sell_date   | date    |
| product     | varchar |
+-------------+---------+
There is no primary key (column with unique values) for this table. It may contain duplicates.
Each row of this table contains the product name and the date it was sold in a market.
```
Write a solution to find for each date the number of different products sold and their names.
The sold products names for each date should be sorted lexicographically.
Return the result table ordered by sell_date.
The result format is in the following example.
```SQL
SELECT 
    sell_date, 
    COUNT(DISTINCT product) AS num_sold,
    GROUP_CONCAT(DISTINCT product ORDER BY product) AS products
FROM Activities
GROUP BY sell_date
ORDER BY sell_date;
```
48. List the Products Ordered in a Period (Easy) 1327.

Table Products and Orders
```
+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| product_id       | int     |
| product_name     | varchar |
| product_category | varchar |
+------------------+---------+
product_id is the primary key (column with unique values) for this table.
This table contains data about the company's products.
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| order_date    | date    |
| unit          | int     |
+---------------+---------+
This table may have duplicate rows.
product_id is a foreign key (reference column) to the Products table.
unit is the number of products ordered in order_date.
```
Write a solution to get the names of products that have 
at least 100 units ordered in February 2020 and their amount.
Return the result table in any order.

```SQL
SELECT product_name, SUM(o.unit) AS unit
FROM Products p
JOIN Orders o
ON p.product_id = o.product_id
WHERE o.order_date BETWEEN '2020-02-01' AND '2020-02-29'
GROUP BY p.product_id, p.product_name
HAVING SUM(o.unit) >= 100;
```
49. Find Users With Valid E-Mails (Easy) 1517.

Table Users
```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| name          | varchar |
| mail          | varchar |
+---------------+---------+
user_id is the primary key (column with unique values) for this table.
This table contains information of the users signed up in a website. Some e-mails are invalid.

```
Write a solution to find the users who have valid emails.
A valid e-mail has a prefix name and a domain where:
The prefix name is a string that may contain letters (upper or lower case),
digits, underscore '_', period '.', and/or dash '-'. The prefix name must start with a letter.
The domain is '@leetcode.com'.
Return the result table in any order.
```SQL
SELECT user_id, name, mail
FROM Users
WHERE mail REGEXP '^[a-zA-Z][a-zA-Z0-9._-]*@leetcode\\.com$';
```
50. Product Sales Analysis III (Medium) 1070.

Table Sales, Product
```
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+
(sale_id, year) is the primary key (combination of columns with unique values) of this table.
product_id is a foreign key (reference column) to Product table.
Each row of this table shows a sale on the product product_id in a certain year.
Note that the price is per unit.

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
product_id is the primary key (column with unique values) of this table.
Each row of this table indicates the product name of each product.

```
Write a solution to select the product id, year, quantity, 
and price for the first year of every product sold.
Return the resulting table in any order.
The result format is in the following example.
```SQL
SELECT
    product_id,
    year AS first_year,
    quantity,
    price
FROM Sales 
WHERE (product_id, year) IN (
    SELECT product_id, MIN(year)
    FROM Sales
    GROUP BY product_id
);
```
