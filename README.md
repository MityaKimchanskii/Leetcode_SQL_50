# Leetcode_SQL_50
SQL 50 

  31. Triangle Judgement
      Table: Triangle

+-------------+------+
| Column Name | Type |
+-------------+------+
| x           | int  |
| y           | int  |
| z           | int  |
+-------------+------+
In SQL, (x, y, z) is the primary key column for this table.
Each row of this table contains the lengths of three line segments.
Report for every three line segments whether they can form a triangle.
Return the result table in any order.
The result format is in the following example.

```SQL

SELECT x, y, z,
  CASE 
    WHEN (x+y > z AND y+z > x AND z+x > y) THEN 'Yes'
    ELSE 'No'
  END AS triangle
FROM Triangle

```


