# HackerRank SQL

Let us start by solving medium and hard SQL problems in HackerRank.  
**Note:** All queries are in T-SQL (Microsoft SQL Server) dialect.

## Level: Medium

**Q.1: Placements:**  
You are given three tables: Students, Friends and Packages. Students contains two columns: ID and Name.
Friends contains two columns: ID and Friend_ID (ID of the ONLY best friend). Packages contains two
columns: ID and Salary (offered salary in $ thousands per month).  

**Solution:** The simplest way to solve this query is using `INNER JOIN` like so:

```sql
select S.name 
from Students as S 
inner join Friends as F 
on S.id = F.id
inner join Packages as P1 
on S.id = P1.id 
inner join Packages as P2 
on F.friend_id = P2.id
where P2.salary > P1.salary 
order by P2.salary;
```

**Q.2: Symmetric Pairs:**  
You are given a table, Functions , containing two columns: X and Y. Two pairs (X , Y ) and (X , Y ) are said to be symmetric pairs if X = Y and X = Y .  
Write a query to output all such symmetric pairs in ascending order by the value of X.

**Solution:**

```sql
select F1.X, 
       F1.Y 
from Functions as F1
inner join Functions as F2 
on F1.X = F2.Y and F1.Y = F2.X
group by F1.X, F1.Y
having F1.X < F1.Y                 -- for cases where X1 != Y1 (i.e. for symmetric pairs 2 24, and 24 2)
       or count(F1.X) > 1          -- for cases where X1 = Y1 (i.e. for symmetric pairs 10 10, and 10 10. Note that there need to be two separate pairs)
order by F1.X;

```

**Q.3: The Report:**  
You are given two tables: Students and Grades. Students contains three columns ID, Name and Marks.  
Grades contains the following data:  
Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty
doesn't want the NAMES of those students who received a grade lower than 8. The report must be in
descending order by grade -- i.e. higher grades are entered first. If there is more than one student with
the same grade (8-10) assigned to them, order those particular students by their name alphabetically.
Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending
order. If there is more than one student with the same grade (1-7) assigned to them, order those
particular students by their marks in ascending order.  
Write a query to help Eve.

```sql
select case when Grade >=8 then Name
       else NULL
       end as "Name",
       Grade,
       Marks
from
(select S.Name,
       case when Marks >= 0 and Marks <= 9 then 1
            when Marks >= 10 and Marks <= 19 then 2
            when Marks >= 20 and Marks <= 29 then 3
            when Marks >= 30 and Marks <= 39 then 4
            when Marks >= 40 and Marks <= 49 then 5
            when Marks >= 50 and Marks <= 59 then 6
            when Marks >= 60 and Marks <= 69 then 7
            when Marks >= 70 and Marks <= 79 then 8
            when Marks >= 80 and Marks <= 89 then 9
            when Marks >= 90 and Marks <= 100 then 10 
       end as Grade,
       S.Marks
from students as S)x
order by Grade desc, Name;

```
