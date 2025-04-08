# L3.1 - SQL Examples
**Example 1: DISTINCT** 
From the classroom relation, find the names of buildings in which every individual classroom has capacity less than 100 (removing duplicates)

```
select distinct building
from classroom
where capacity < 100;
```

**Example 2 AND**:
Find the list of all students of departments which have a budget < 0.1M
```
select name, budget
from student, department
where student.dept_name = department.dept_name 
and
budget < 1000
```
**Example 3: AS**
Same query in the previous slide can be framed using AS
```
select S.name as studentname, budget as deptbudget
from student as S, department as D
where S.dept_name = D.dept_name and budget < 100000;
```
**Example 4: ORDER BY**
Obtain the list of all students in alphabetic order of departments and within department, decreasing order of total credits
```
select name, dept_name, tot_cred
from student
order by dept_name ASC, tot_cred DESC;
```
**Example 5: UNION**
```
select course_id 
from teaches 
where semester = 'fall'
and year = 2018

union

select course_id
from teaches
where semester='spring'
and year = 2018
```
**Example 6: INTERSECT**
```
query1
intersect
query2
```
**Example 7: AVG**
```
select building, avg(capacity)
from classroom
group by building
having avg(capacity) > 25
```

#  L3.2 - Intermediate SQL
## Nested Subqueries
A subquery is a *select-from-where* expression that is nested within another query
```
select A...
from r...
where B <operation> (subquery)
```


## Subqueries in the WHERE clause
Typical use of subqueries is to perform tests:
* for set memberships
* For set comparisons
* For set cardinality
Example:
```
select distinct course_id
from section
where semester = 'Fall' and year = 2019
and
course_id in (select course_id from section where semester = 'spring' and year = 2010)
```

### SOME clause
```
# Find names of instructors with salary greater than tha tof some (at least one) instructor in the biology department
# USING NORMAL
select distinct T.name
from instructor as T, instructor as S
where T.saalary > S.salary and S.dept_name = 'Biology'

# Using some clause
select name
from instructor
where salary > some (select salary from instructor where dept_name = 'Biology')
```

```
F <comp> some r <=> t such that (F <comp> t)
```
* Basically sees that at least one is true

### ALL clause
```
F <comp> some r <=> t such that (F <comp> t)
```
* All the elements of that tuple, the condition must be true

### EXISTS clause
* Return value true if argument subquery is nonempty
Example:
```
select course_id
from section as S
where semester = 'Fall' and year = 2009 and 
exists (select * 
		from section as T
		where semester = 'spring' and year = 2010
		and S.course_id = T.course_id)
```
* Correlation name - variable S in outer query
* Correlated subquery - inner query

![[Pasted image 20250409023349.png]]

### Unique clause
constructs tests whether a subquery has any duplicate tuples in its result

### HAVING vs WHERE clause:
![[Pasted image 20250409024430.png]]


## Subqueries in the FROM Clause
* SQL allows a subquery expression to be used in the from clause
**Example:** Find the average instructors' salaries of those departments where the average salary is greater than 42,000
```
select dept_name, avg_salary
from (select dept_name, avg(salary) as avg_salary
		from instructor
		group by dept_name)
where avg_salary > 42000;

#Another way
select dept_name, avg_salary
from (select dept_name, avg(salary)
		from instructor
		grou pby dept_name) as dept_avg(dept_name, avg_salary)
where avg_salary > 42000;
```

### WITH clause
Provides a way of defining temporary relation whose defition is available only to the query in which the with clause occurs
```
# Find all departments with ehe maximum budget
with max_budget(value) as 
		(select max(budget) 
		from department)
select department.name
from department, max_budget
where department.budget = max_budget.value;

```

#### Complex Queries
Find all departments where the total salary is greater than the average of the total salary at all departments
```
with dept_total(dept_name, value) as
			select dept_name, sum(salary)
			from instructor
			group by dept_name,
	 dept_total_avg(value) as
			(select avg(value)
			from dept_total)
select dept_name
from dept_total, dept_total_avg
where dept_total.value > dept_total_avg.value;
```

## Scalar Subquery
* Where single value is expected
**Example** : List all departments along with the number of instructors in each department
```
select dept_name, (select count(*)
					from instructor
					where departments.dept_name = instructor.dept_name)
as num_instructors
from department
```

ghghg

## Deletion
**Example**: Delete all instructors whose salary in less than average salary of instructors
```
delete from instructor
where salary < (select avg(salary)
				from instructor);
```

## Insertion
**Example**: Add all instructors to the student relation with tot_creds set to 0
```
insert into student
select ID, name, dept_name, 0
from instructor
```

## UPDATE
**Example**: Increase salaries of instructors whose salary is over 100k by 3%, and all others by a 5%
```
update instructor
set salary = salary * 1.03
where salary > 100000;
update instructor
set salary = salary * 1.05
where salary <= 100000;
```
^ inefficient, can use case statement

## CASE clause
```
update instructor
set salary = case
			where salary <= 1000
			then salary * 1.05
			else salary * 1.03
			end
```

## Updates with scalar subqueries
