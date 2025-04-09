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

# L3.3 - Intermediate SQL p2
## Join operation
Join operation is basically a cartesian product
Types of join
* Cross Join
* Inner Join
	* Equi-join
		* Natural Join
* Outer Join
	* Left outer join
	* Right outer join
	* full outer join
* self join
### Cross Join
```
select * from employee cross join department
#same as from employee, department (Implicit)
```
### Inner join
Joined by columns, like cartesian product, where the condition is met, is retained, 
* Missing tuples are missing

### Outer Join
Like inner join but adds missing tuples by replacing the columns with null 

#### Left outer join
![[Pasted image 20250409125617.png]]
```
course natural left outer join prereq
# A = course; B = prereq
# Vice versa for right
```
#### Right outer join

![[Pasted image 20250409125957.png]]

#### Full Outer Join
![[Pasted image 20250409130023.png]]

### Joined relations
* Joined operations take two relations and return a result another relation
* These additional operations are typically used as a subquery expressions in the from clause
* **Join condition**: defines which tuples in the two relations match, and what attributes are present in the result of the join
* **Join type**: defines how tuples in each relation that do not match any tuple in the other relation are treated

### Examples
```
course inner join prereq on
course.couse_id = prereq.course_id
```

## Views
* some cases it is not desirable for all users to see the entire logical model
* **A view** provides a mechanism to hide certain data from the view of certain users
* Any relation that is not of the conceptual model but is made visible to a user as a 'virtual relation' called a view
View is defined as
```
create view v as <query expression>
# Where <query expression> is any legal SQL expression, 
# View name represented by v
```
* Once a view is defined, the view name can be used to refer to virtual relation that the view generates
* View definition is not the same as creating a new relation by evaluating the query expression
	* Rather a view definition causes the saving of an expression, the expression is substituted into queries using the view
### Examples
* A view of instructors without their salary
```
# creating the faculty view
# Acts as another relation itself
create view faculty as
	select ID, name, dept_name
	from instructor

# find all the instructors in the Biology department
select name 
from faculty
where dept_name = 'biology'
```
* everytime the view query is run, the view initialization query is also run, so it's updated for every query
**Example 2**
* View of department salary totals
```
create view department_total_salary(dept_name, total_salary) as
	select dept_name, sum(salary)
	from instructor
	group by dept_name
```
**Example 3:** Making views using other views
```
create view a as <>

create view b as
	select <column>
	from a
	where <condition>
```
### Views defined using other views
* one way may be used in the expressino defining another view
* A view relation v1 is said to depend directly on a view relation v2 if v2 is used in the expression defining v1
* A view relation v is said to recursive if it depends on itself

### View expansion
* A way to define the meaning of views defined in terms of other views
* Let view v1 be defined by an expression e1 that may itself contain uses of view relations
* View expansion of an expression repeats the following replacement step:
```
repeat 
	find any view relation vi in e1
	replace the view relation vi by the expression defining vi
until no more view relations are present in e1
# As long as view definition are not recursive, this loop will terminate
```
### Updating view
* Adding a new tuple faculty view which we defined earlier
```
insert into faculty values(a, b, c)
# Insertion must be represented by the insertion of the tuple into the instructor
```
**Most SQL implementations allow updates only on simple views**
* FROM clause has only one database relation
* select clause only contains only attribute names of the relation and no subqueries
* Any attribute not listed in select can be set to null
* query does not have a group by or having clause

### Materialized views:
Creating a physical table containing all the tuples in the result of the query defining the view
if relations used in the query are updated, the materialized view result becomes out of date, needs to be updated



# L3.4 - Intermediate SQL p3
## Transactions
* unit of work
* Atomic
	* Either fully executed or rolled back
* Isolation from concurrent transactions
* Begin implicitly
	* End by commit or rollback
* Default on most DB: Each SQL statement commits automatically
	* Can turn off autocommit from a session
## Integrity Constraints
* Guard against accidental damage to the database, by ensuring that authorized changes to the database don't result in a loss of data consistency
* On a single relation:
	* Not null
	* Primary Key
	* Unique
	* Check(P)
**Example**:
```
create table section (
semester varchar(6),
check (semester in (<list of semesters>))
)
```
## Referential Integrity
* Ensures that a value that appears in one relation for a given set of attributes laso appears for a certain set of attributes in another relation
	* eg: If biology exists for teacher, biology exists in department
### Cascading
```
# No cascading
create table course (
dept_name references department
)

# with cascading
create table course (
dept_name,
foreign key(dept_name) references department
on update cascade
on delete cascade
)

```
## Datatypes in SQL
* date
* time
* timestamp
* interval

## Index Creation
```
create index studentID_index on student(ID)
```
Index helps speed up access to records

## User Defined Types 
```
create type Dollars as numeric(12, 2) final
```
## Domains
```
create domain name type
```
* Types and domains are similar
* Domains can have contraints on them
```
create domain degree_level varchar(10)
constraint degree_level)test
check (value in (degress))
```

## Large Object types
* photos videos cad etc 
	* BLOB - binary large object - interpretation left to an applicatio outside db
	* clob - character large object -
* When a query returns a large object, a pointed is returned rather than the large object itself
## Authorization
Forms of authorization on parts of the databse:
* READ
* INSERT
* UPDATE
* DELETE
Form of authorization to modify the database schema
* INDEX
* RESOURCES
* ALTERATION
* DROP

### Granting Authorization
* grant statement
```
grant <privilege list>
on <relation name or view name>
to < user list>
```
### Revoke Authorization
```
revoke <privilege list>
on < relation name or view name>
from <user name>
```
* If revokee list includes public, all users lose the privilege
* If same privilege granted twive to user by different grantees, the user may retain the privilege after the revocation
* 
### Privilege List
* SELECT
* INSERT
* MODIFY
* ALL

## Roles
```
creat role <role name>

# privileges can be granted to roles
grant select on takes to instructor

# roles can be granted to users as wel as other rles
create role teaching_assistant
grant teaching_assistant to instructor
	* Instructor inherits all privileges of teaching assistant

# Chain of roles
creat role dean
grant instructor to dean
grant dean to satoshi
``` 

## Authorization on Views
```
create view geo_instructor as
(select * 
from instructor
where dept_name = 'Geology')
grant select on geo_instuctor to geo_staff
```

## Other features of Authorization
```
# Transfer of Privilegees
grant select on department to Amit with grant option

revoke select on department from amity, satoshi cascade

revoke select on departfrom amit, satochi restrict
```

# L3.5 - Advanced SQL
## Functional and Procedural Constraints 
**Model 1**
![[Pasted image 20250409145551.png]]
**Model - 2:** embedded as a part of a native language
![[Pasted image 20250409145609.png]]

* Functional/Procedurals can be written in SQL itself or in an external language
* External are useful with specialized data types such as images and geometrid=c objects
* Some database systems support table-valued function, which can return a relation as a result

## SQL function
**Example**: Define a function that, given the name of a department, returns the count of the member of instructors in that department
```
# Creating Function
create function dept_count(dept_name varchar(20))
	returns integer
	begin
	declare d_count integer
		select count(*) into d_count
		from instructor
		where instructor.dept_name = dept_name
	return d_cont;
	end

# Using in a query (departments with more than 12 instructors)
select dept_name, budget
from department
where dept_count(dept_name) > 12
```

### Parts of function
* Compound statement:  (**begin** ... **end**)
* May contain multiple SQL statements between begin and end
* Local variables can be declared within  a compound statemt
* returns - variable type
* return - specify what to return
Functions are parameterized views (in views, everything must be defined, here it can be variable)

### Returning Tables
```
create function <name>(<input types and vars>)
	returns table (<Data types and variables)
	return table(<query statement with matching output variables>)

# Using
select *
from table(instructor_of('music'))
```

### SQL Procedures
```
create procedure dept_count_proc (in dept_name varcher(20), out d_count integer)
	begin
		select count(*) into d_count
		from instructor
		where instructor.dept_name = dept_count_proc.dept_name
	end
```
* Procedures can be invoked either from an SQL procedure of from embedded SQL, using the call statement
```
declare d_count integer
call dept_count_proc('Physics', d_count)
```
* SQL allows function ovrloading (with different argument types)

### Loops (While, Repeat and For)
```
# while loop
while loop
	while <boolean expression> do
		<sequence of statements>
	end while

# Repeat
repeat loop
	repeat
		<sequence of statements>
	until <boolean expression>
	end repeat
	
# For Loop
declare n integer default 0;
for r as 
	select budget from department
do
	set n = n + r.budget
endfor;
```
### Conditional (if-then-else/case)
```
# If-then-else
if <boolean-expression> then
	<sequence of statements>;
elseif <boolean-expression> then
	<sequence of statements>;
else
	<sequnce of statements>;
endif;

# case (type 1)
case variable
	when <value> then
		<sequence of statements>
	...
	else
		<sequence of statements>
end case;

# case (type 2)
case
	when sql-expression = value1 then
		sequence of statements
	..
	else

end case
```
### Exception
```
declare <variable> condition
declare exit handler for <variable>
being
	signal
end
```
* Handler here is exit, causes enclosing begin and end to be terminate adn exit

### External Language Routines
* Allows definition of functions in an imperative programming lanugage
* Such functions can be more efficient than functions in SQL
```
# Procedure from C
create procedure dept_count_proc(
		in dept_name varchar(20),
		out count integer
		)
	language C
	external name'/user/avi/bin...'

# function from C
create function dept_name(dept_name varchar(20))
returns integer
	language C
	external name '/path'
```

#### Benefits:
* More effieicnt for many operations, more expressive power
#### Drawbacks:
* Code to implement function may need to be loaded into databse system and executed in the database system address space
* Rick of accidental corruption of database structures
* Security rick, allowing users access to unauthorized data
* Alternative, which give good security at cost of performance

#### Security
* Use sandbox technique
* Run external language function in a separate process with no access to database process memory
* Both ahve performance overheads

## Trigger
* Trigger defines a set of actions that are performed in response to an insert, update or delete
	* When SQL operatino is executed, trigger is said to have been activated
	* Triggers are option
	* defined using the create trigger statement
### Uses
* Enforce data integrity via referential constraints and check constraints
* To cause updates to other tables, automatically generate or transform values for inserted or updated rows, or invoke functions to perform tasks such as issuing alerts

### Design
* Specify the events
* Specify the time
* Specify the actions to be taken

### Types
* BEFORE triggers
	* Run before and Update or insert
	* Values that rae being updated or inserted can be modified before the database is actually modified
* BEFORE DELETE triggers
	* run before a delete
* AFTER triggers
	* Run after an update, insert and delete
	* You can use triggers that run after an update or insert to:
		* Update data in other tables
			* Useful for maintain relationship
		* Check against other data in the table or in other tables
			* Useful to ensure integrity when referential integrity constaints aren't appropriate
		* Run non-database operations coded in user-defined functions
			* Useful when issuing alerts or to update information outside database
#### Row level and Statement Level Triggers
* Row level: When row is affected
* Statement Level: Perform a single action for all rows affected by a statement, instead of executing a separate action for each affected row
	* Used for each statement instead of each row
	* Uses referencing old table or referencing new table to refer to summary tables called transition tables containing the affected rows
	* Can be more efficient when dealing with sql statements that update large number of rows

### Events and actions in SQL
* Can be an inser, delete or update
* * Restricted to specific attributes
* Values of attributes before and after an update can be referenced
	* referencing old row as: for deletes and updates
	* referencing new row as: for inserts and updates
* Triggers can be activated before an event
```
create trigger setnull_trigger before update of takes
referencing new row as nrow
for each now 
when (nrow.grade = '')
	begin atomic
		set nrow.grade = null;
	end
```

### How to use triggers
* Optimal use of DML triggers is short, simple and easy to maintain
* Tyical and recommended uses:
	* Logging changes
	* auditing users and their actions
	* adding additional values to a table
	* Simple validation
### risks:
* too many
* complex code
* cross-server
* triggers calling triggers
* recursive triffers are on
* functions, stored procedures, or views are in triggers
* iteration occurs