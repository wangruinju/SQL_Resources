---
title: "Easy/Medium/Hard"
author: "Rui Wang"
date: "1/16/2017"
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
613. Shortest Distance in a Line

```sql
-- Write your MySQL query statement below
select min(abs(p1.x - p2.x)) as shortest
from point p1, point p2
where p1.x > p2.x
order by shortest
limit 1
```

627. Swap Salary

```sql
UPDATE salary
    SET sex  = (CASE WHEN sex = 'm' 
        THEN  'f' 
        ELSE 'm' 
        END)
```
584. Find Customer Referee

```sql
select name
from customer
where referee_id is null or referee_id <> '2'
```

620. Not Boring Movies

```sql
-- Write your MySQL query statement below
select * from cinema
where mod(id, 2) = 1
and description <> "boring"
order by rating desc
```
610. Triangle Judgement

```sql
select *,
case when x+y <= z or y+z <= x or x+z <= y then 'No'
     else 'Yes' end as triangle
from triangle
```
586. Customer Placing the Largest Number of Orders

```sql
select customer_number
from orders
group by customer_number
order by count(*) desc
limit 1
# tie case
# select a.customer_number
# from
# (select customer_number, count(*) as cnt
# from orders
# group by customer_number) a
# where a.cnt == (select count(*) as cnt from orders group by customer_number order by cnt desc limit 1)
```
603. Consecutive Available Seats

```sql
select distinct c1.seat_id
from cinema c1, cinema c2
where c1.free = 1 and c2.free = 1
and (c1.seat_id - 1 = c2.seat_id or c1.seat_id + 1 = c2.seat_id)
order by c1.seat_id
```
577. Employee Bonus

```sql
select name, bonus
from Employee e
left join Bonus b
on e.empId = b.empId
where bonus is null or bonus < 1000
```
607. Sales Person

```sql
select name
from salesperson
where sales_id not in
    (
    select sales_id
    from orders o
    join company c
    on o.com_id = c.com_id
    where c.name = 'RED'
    )
```

597. Friend Requests I: Overall Acceptance Rate

```sql
select round(
ifnull(
(select count(*) from (select distinct requester_id, accepter_id from request_accepted) a)
/
(select count(*) from (select distinct sender_id, send_to_id from friend_request) b)
, 0)
, 2) as accept_rate
```

619. Biggest Single Number

```sql
select max(num) as num
from (
select num
from number
group by num
having count(*) = 1
) a
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
570. Managers with at Least 5 Direct Reports

```sql
select Name from Employee
where Id in (
select distinct ManagerId
from Employee
where ManagerId is not null
group by ManagerId
having count(ManagerId) >= 5
)
```

608. Tree Node

```sql
select id,
case
when p_id is null then 'Root'
when id in (select distinct p_id from tree) then 'Inner'
else 'Leaf' end as Type
from tree
order by id
```

612. Shortest Distance in a Plane

```sql
select round(sqrt(power(p1.x-p2.x, 2)+power(p1.y-p2.y, 2)), 2) as shortest
from point_2d p1, point_2d p2
where p1.x <> p2.x or p1.y <> p2.y
order by shortest
limit 1
```

585. Investments in 2016

```sql
select round(sum(TIV_2016), 2) as TIV_2016
from insurance
where
TIV_2015 in (select TIV_2015 from insurance group by TIV_2015 having count(*) > 1)
and (LAT, LON) in (select LAT, LON from insurance group by LAT, LON having count(*) = 1)
```
602. Friend Requests II: Who Has the Most Friends

```sql
select id, count(*) as num
from
    (select requester_id as id
    from request_accepted
    union all
    select accepter_id
    from request_accepted) a
group by id
order by num desc
limit 1
```

580. Count Student Number in Departments

```sql
select dept_name,
ifnull(count(distinct student_id), 0) as student_number
from department d
left join student s
on d.dept_id = s.dept_id
group by d.dept_id
order by student_number desc, dept_name
```
574. Winning Candidate

```sql
select Name
from Candidate c
join
    (
    select CandidateId, count(*) as cnt
    from Vote
    group by CandidateId
    order by cnt desc
    limit 1
    ) a
on c.id = a.CandidateId
```

578. Get Highest Answer Rate Question

```sql
select question_id as survey_log
from
    (
    select question_id, count(answer_id)/count(question_id) as rate
    from survey_log
    group by question_id
    order by rate desc
    limit 1
    ) a
```

614. Second Degree Follower

```sql
select f1.follower, count(distinct f2.follower) as num
from follow f1
join follow f2
on f1.follower = f2.followee
group by f1.follower
order by f1.follower, num desc
```
618. Students Report By Geography

```sql
set @r1 = 0;
set @r2 = 0;
set @r3 = 0;

select
America.name as America, Asia.name as Asia, Europe.name as Europe
from
(select name, @r1:= @r1+1 as id from student where continent = 'America' order by name) America
left join
(select name, @r2:= @r2+1 as id from student where continent = 'Asia' order by name) Asia
on
America.id = Asia.id
left join
(select name, @r3:= @r3+1 as id from student where continent = 'Europe' order by name) Europe
on
America.id = Europe.id
```

571. Find Median Given Frequency of Numbers

```sql
select avg(n1.Number) as median
from Numbers n1
where n1.Frequency >= abs(
    (select sum(n2.Frequency) from Numbers n2 where n1.Number >= n2.Number)
    -
    (select sum(n2.Frequency) from Numbers n2 where n1.Number <= n2.Number)
)
```

178. Rank Scores

```sql
-- Write your MySQL query statement below
select s1.Score, 
(select count(distinct  s2.Score) from Scores s2 where s2.Score >= s1.Score) as Rank
from Scores s1
order by s1.Score desc;
```

569. Median Employee Salary

```sql
select e1.*
from Employee e1, Employee e2
where e1.Company = e2.Company
group by e1.Company, e1.Salary
having sum(case when e1.Salary = e2.Salary then 1 else 0 end)
    >= abs(sum(sign(e1.Salary-e2.Salary)))
order by e1.Company, e1.Salary, e1.Id
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

615. Average Salary: Departments VS Company

```sql
select a.pay_month, a.department_id,
case when a.avg_mon_dept_amount = b.avg_dept_amount then 'same'
when a.avg_mon_dept_amount > b.avg_dept_amount then 'higher'
else 'lower' end as comparison
from
(select Date_Format(pay_date, '%Y-%m') as pay_month, department_id, avg(amount) as avg_mon_dept_amount
from salary s
join employee e
on s.employee_id = e.employee_id
group by pay_month, department_id) a
join
(select Date_Format(pay_date, '%Y-%m') as pay_month, avg(amount) as avg_dept_amount
from salary s
join employee e
on s.employee_id = e.employee_id
group by pay_month) b
on a.pay_month = b.pay_month
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

579. Find Cumulative Salary of an Employee

```sql
select e1.Id, e1.Month, sum(e2.Salary) as Salary
from Employee e1, Employee e2
where
e1.Id = e2.Id
and e1.Month - e2.Month >= 0
and e1.Month - e2.Month < 3
and (e1.Id, e1.Month) not in (select Id, max(Month) from Employee group by Id)
group by e1.Id, e1.Month
order by e1.Id, e1.Month desc
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
