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

**Q.5: Occupations:**

**Solution:** We can write this query in multiple ways. One of the ways is using Window functions and full outer joins to merge the subqueries.  

```sql
Select D.Name as Doctor_name,
       P.Name as Professor_name,
       S.Name as Singer_name,
       A.Name as Actor_name
from(
    (Select Name,
            row_number() over (partition by occupation
                               order by name) as id
     from Occupations
     where Occupation = 'Doctor') as D
    full outer join
    (Select Name,
            row_number() over (partition by occupation
                               order by name) as id
     from Occupations
     where Occupation = 'Professor') as P
    on D.id = P.id
    full outer join
    (Select Name,
            row_number() over (partition by occupation
                               order by name) as id
     from Occupations
     where Occupation = 'Singer') as S
    on P.id = S.id
    full outer join
    (Select Name,
            row_number() over (partition by occupation
                               order by name) as id
     from Occupations
     where Occupation = 'Actor') as A
    on S.id = A.id
   );
```

Another way to solve this query is using the `pivot` function in T-SQL. However, in the above query I have tried sticking to using standard SQL.

**Q.6: Binary Tree Nodes:**

**Solution:** We can write this query using simple case when statements.  
Note that all the nodes (in the tree) are already mentioned in the N column in the table.  

```sql
select N as Node,
       (case when P is null then 'Root'
             when N in (select distinct P from bst where P is not null) then 'Inner'
             when N not in (select distinct P from bst where P is not null) then 'Leaf'
        end) as Role
from bst
order by N;
```

**Q.7: New Companies:**

**Solution:** This query can be solved using `count` aggregate function and simple `inner join` on all the tables.  

```sql
select C.company_code,
       C.founder,
       count(distinct L.lead_manager_code),
       count(distinct S.senior_manager_code),
       count(distinct M.manager_code),
       count(distinct E.employee_code)
from Company as C
inner join Lead_Manager as L
on C.company_code = L.company_code
inner join Senior_Manager as S
on L.lead_manager_code = S.lead_manager_code
inner join Manager as M
on S.senior_manager_code = M.senior_manager_code
inner join Employee as E
on M.manager_code = E.manager_code
group by C.company_code, C.founder
order by C.company_code, C.founder;
```
