# Leetcode_SQL_50

  31. Triangle Judgement
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

  32. Consecutive Numbers
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

  33. Product Price at a Given Date
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

SELECT t1.person_name
FROM Queue t1
JOIN Queue t2 ON t1.turn >= t2.turn
GROUP BY t1.turn
HAVING SUM(t2.weight) <= 1000
ORDER BY SUM(t2.weight) DESC
LIMIT 1

```
 35. Last Person to fit in the Bus
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

36. Count Salary Categories
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

