# HackerRank SQL

Let us start by solving medium and hard SQL problems in HackerRank.  
**Note:** All queries are in T-SQL (Microsoft SQL Server) dialect.

## Level: Medium

**Q.1: Placements:**  

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

**Solution:** Here we can use the `case when` function to generate the report as per the requirement.

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

**Q.4: The PADS:**

**Solution:** Here we can write two queries as below making use of the string functions/operators in SQL.

```sql
select name + '(' + left(occupation, 1) + ')'
from occupations
order by name;

select 'There are a total of', count(occupation), lower(occupation) + 's.'
from occupations
group by occupation
order by count(occupation), occupation;
```
