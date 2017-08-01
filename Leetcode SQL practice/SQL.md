---
title: "Easy"
author: "Rui Wang"
date: "8/1/2017"
output: html_document
---

595. Big Countries

```sql
-- Write your MySQL query statement below
select 
    name,
    population,
    area
from World
where area >= 3000000 or population >= 25000000
```

627. Swap Salary

```sql
-- Write your MySQL query statement below
UPDATE salary
    SET sex  = (CASE WHEN sex = 'm' 
        THEN  'f' 
        ELSE 'm' 
        END)
```

620. Not Boring Movies

```sql
-- Write your MySQL query statement below
select * from cinema
where mod(id, 2) = 1
and description <> "boring"
order by rating desc
```

182. Duplicate Emails

```sql
-- Write your MySQL query statement below
SELECT Email FROM Person 
GROUP BY Email 
HAVING COUNT(Email)>1
```

175. Combine Two Tables

```sql
-- Write your MySQL query statement below
SELECT Person.FirstName, 
Person.LastName, 
Address.City, 
Address.State 
from Person 
LEFT JOIN Address 
on Person.PersonId = Address.PersonId;
```

181. Employees Earning More Than Their Managers

```sql
-- Write your MySQL query statement below
select e1.name as Employee
from Employee e1, Employee e2
where e1.ManagerId = e2.Id and e1.Salary > e2.Salary;
```

183. Customers Who Never Order

```sql
-- Write your MySQL query statement below
select Name as Customers
from Customers
where Id not in (select CustomerId from Orders);
```

197. Rising Temperature

```sql
-- Write your MySQL query statement below
select w1.Id
from Weather w1, Weather w2
where ( DATEDIFF(w1.Date, w2.Date)=1 )
and w1.Temperature > w2.Temperature;
```

596. Classes More Than 5 Students

```sql
-- Write your MySQL query statement below
select class 
from courses 
group by class 
having count(distinct student)>=5;
```

196. Delete Duplicate Emails

```sql
-- Write your MySQL query statement below
delete p1
from Person p1, Person p2
where p1.Email = p2.Email
and p1.Id > p2.Id;
```

176. Second Highest Salary

```sql
-- Write your MySQL query statement below
select
max(Salary) as SecondHighestSalary
from Employee 
where Salary < (select Max(salary) from Employee);
```

178. Rank Scores

```sql
-- Write your MySQL query statement below
select s1.Score, 
(select count(distinct  s2.Score) from Scores s2 where s2.Score >= s1.Score) as Rank
from Scores s1
order by s1.Score desc;
```

180. Consecutive Numbers

```sql
-- Write your MySQL query statement below
select distinct l1.Num as ConsecutiveNums
from Logs l1, Logs l2, Logs l3
where l1.ID - 1 = l2.ID
and l1.ID - 2 = l3.ID
and l1.Num = l2.Num
and l1.Num = l3.Num;
```

184. Department Highest Salary

```sql
-- Write your MySQL query statement below
select d.Name as Department, e1.Name as Employee, e1.Salary 
from Department d, Employee e1
where d.Id = e1.DepartmentId
and (DepartmentId, Salary) in
(select e2.DepartmentId, max(e2.Salary) as max_salary from Employee e2 group by e2.DepartmentId) ;
```

177. Nth Highest Salary

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  set N = N-1;
  RETURN (
      -- Write your MySQL query statement below.
      select distinct Salary from Employee order by Salary desc limit N,1
  );
END
```

601. Human Traffic of Stadium

```sql
-- Write your MySQL query statement below
SELECT t.* FROM stadium t
    LEFT JOIN stadium p1 ON t.id - 1 = p1.id
    LEFT JOIN stadium p2 ON t.id - 2 = p2.id
    LEFT JOIN stadium n1 ON t.id + 1 = n1.id
    LEFT JOIN stadium n2 ON t.id + 2 = n2.id
WHERE (t.people >= 100 AND p1.people >= 100 AND p2.people >= 100)
     OR (t.people >= 100 AND n1.people >= 100 AND n2.people >= 100)
     OR (t.people >= 100 AND n1.people >= 100 AND p1.people >= 100)
ORDER BY id;
```

185. Department Top Three Salaries

```sql
-- Write your MySQL query statement below
select d.Name Department, e1.Name Employee, e1.Salary
from Employee e1 
join Department d
on e1.DepartmentId = d.Id
where 3 > (select count(distinct(e2.Salary)) 
                  from Employee e2 
                  where e2.Salary > e1.Salary 
                  and e1.DepartmentId = e2.DepartmentId);
```

262. Trips and Users

```sql
-- Write your MySQL query statement below

select Request_at as Day, 
round(sum(case when Status like 'cancelled_%' then 1 else 0 end) / count(*),2) as 'Cancellation Rate'
from Trips 
join Users
on Users_Id = Client_Id
where Banned = 'No'
and Request_at between '2013-10-01' and '2013-10-03'
group by Request_at;
```