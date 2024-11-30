# Leetcode_SQL_50

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


