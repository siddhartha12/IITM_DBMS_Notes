# L4.1 - Formal Relational Query Language p1

## Relational Algebra
[[DBMS Week 2 notes#L2.2]]

## Select Operation
𝜎<sub>p</sub> (r)
p -> selection predicate

𝜎<sub>p</sub> (r) = {t|t *E* r and p(t)}

where p is a formula in propositional calculus consisting of the terms:
* ^ - and
* V - OR
* ¬ (not)

Each terms is one of :
```
<attribute> op <attribute> or <constant>
op -> operator
```
Example
𝜎<sub>dept_name = 'Physics'</sub> (instructor)

SQL Equivalent to
```
select *
from instructor
where dept_name = 'Physics'
```

### Projection Operator
∏<sub>A1, A2...Ak</sub> (r)

this is the equivalent of 
```
select distinct A1, A2,..., Ak
from r
```

### Difference operator

### Union Operator

### Intersection operator

### Rename operator
(rho)

### Division Operator
* Applied to two relations
* R(Z) / s(X) where X subset Z.
	* Let Y = Z-X
	* Let y be the set of attributes of r that are not attributes of S
* The result of division is a relation T(Y) that includes tuple t if it appears in:
	* R with t(Y) = t
	* S with t(X) = t
* For a tuple to appear in the result T of the DIVISION, the values in t must appear in R in combination with every tuple in S

![[Pasted image 20250410154406.png]]

**Example - 1**
![[Pasted image 20250410154502.png]]

**Example - 2**
![[Pasted image 20250410154549.png]]

# L4.2 - Formal relational languages p2

Formal relational query languages:
* Relational Algebra
	* procedural and algebra based
* Tuple relational calculus
	* Non-procedural and Predicate Calculus Based
* Domain Relational Calculus
	* Non-procedural and predicate calculus based

## Predicate logic
* Extensions of propositional logic or Boolean Algebra
* Adds concepts of predicates and quantifiers to better capture the meaning of statements that cannot be adequately expressed by propositional logic
Example:
* Statement x greater than 3 represented by P(x) which gives boolean value

### Universal Quantifiers
* Where a P is true for all values of X
* ⱯP(x) used to denote it
* Ɐ is the universal quantifier
**Example**:
P(x) : X +2 > X

### Existential Quantification
Mathematical statements assert that there is an element with a certain property. Expressed by existential quantification. Can be used to form a proposition that is true if and only if P(x) is true for at least one value of x in the domain

* Formally, the existential quantificationof P(x) is the statement "There exists an element x in the domain such that P(x)"
* Notation: ƎP(x) 
* Ǝ is called the existential quantifier

## Predicate Calculus Formulas
* Set of attributes and constant
* Set of comparison operators <= >= = >
* Set of connectives ^ V
* Implication =>
* Set of quantifiers

## Tuple Relational Calculus
TRC is a non-procedural query language, each query of the form

								{t|P(t)}

t -> resulting tuples
P(t) = known as predicate and there are condition that are used to fetch it, may have various conditional logically combined with operators

### Quantifiers
Ǝt *E* r(Q(t)) -> there exists a tuple in t in relation r such that predicate Q(t) is true
Ɐt *E* r(Q(t)) -> Q(t) is true 'for all' tuples in relation r

**Example**:
```
{P| ƎS ∈ Students and (S.cgpa > 8 ^ P.name = S.name ^ P.age = S.age)}

# Returns the name and age of students with a CGPA above 8
```

### TRC Examples
**Example:** Relational schema
* student(rollNo, name, year, courseId)
* course(courseId, cname, teacher)
Q: Find out the names of all studeents who have taken the course DBMS
```
{t| ƎS ∈ Students ƎC ∈ course(s.courseId = c.courseId ^ c.cname = 'DBMS')}
{s.name | S ∈ Students ^ ƎC ∈ course(s.courseId = c.courseId ^ c.cname = 'DBMS') }
```

### Safety of Expression
* It is possible to write tuple calculus expressions that generate infinite relations
* To guard against the problem, we restrict the set of allowable expression to safe expressions

## Domain Relational Calculus
Non procedural query language equivalent in power to the tuple relational calculus
* EAch query an expression of the form:

		{<x1, x2, xn> | P(x1, x2 ... xn)}

## Comparison:
### Select:
![[Pasted image 20250410164637.png]]

### Project
![[Pasted image 20250410164701.png]]

### Combining
![[Pasted image 20250410164805.png]]

### Intersection
![[Pasted image 20250410164854.png]]

### Natural Join
![[Pasted image 20250410164914.png]]

### Division
![[Pasted image 20250410164940.png]]

# L4.3 - Entity Relationship Model
What is design:
* Satisfies a given informal functional specification
* Conforms to limitations of the target medium
* Mets requirements
* satisfied criteria
* satisfied restrictions

Abstrctions procudes the majot tool to handle disorganized complexity by cunking information
eg: binary to octa or hex

## Database Design
* Requirement Analysis
* Database Designing
* Implementation

### Approach
* Logical Model
	* Business ecision
	* Computer decision
* Physical Model: Deciding on the physical layout of the dataabse

### Logical Model
* Models an enterprise as a collection of entities and relationships
* Database normalization

### ER Model
* Was developed to facilitate db design 
* Breaks real world meaning and interactions into conceptual schema

#### Attributes
* Properly associated with an entity/set, based on it, an eitty can be identified uniquely
* Types:
	* Simple, composite
	* Single value, multi value
	* Derived
* Domain: set of permitted values for each attribute

#### Entity sets
* entity is an object that exists and is distinguishable from other objects
* set is a set of entities of same type sharing properties
* Entity represented by set of attributes
* primary key subset of attributes

#### Relationship Sets
![[Pasted image 20250410170444.png]]

#### Degree
Binary relationships = degree 2
More than 2 are rare

#### Redundancy
![[Pasted image 20250410170810.png]]

#### Entity sets
2 types:
* Strong Entity
	* uniquely identifiable
* Weak eneity
	* no primary key, not uniquely identifiable
	* partial key called discriminator
	* Cannot independently exist in the ER model

# L4.4 - ER2
Entities - Rectangles
Weak entity - double rectable
Relationships - Diamonds (possibly with attributes)
Roles:
![[Pasted image 20250410172024.png]]
One to many:
![[Pasted image 20250410172121.png]]
Many to many:
![[Pasted image 20250410172113.png]]


![[Pasted image 20250410172157.png]]

More complex constraints:
* A line may have associated minimum and maximum cardinality, showin the form l..h
	* l -> minimum
		* l = 1 -> total participation
	* h -> maximum
		* h = 1 -> entity participates in at more one relationship
		* h = * indicates no limit
	![[Pasted image 20250410173243.png]]
	

## Reduction to Relation Schema
* Entity sets and relationship sets can be expression uniformly as relation schemas that represent the content of the database

## Cardinality Constraints on Ternary Relationship
* We allow at most one arrow out of a ternary (or greater degree) relationship to indicate a cardinality constraint
* If there is more than one arrwo, there are two ways of defining the meaning
	* R b/w A, B C
		* A associated with a unique entity from B and C 
		* Each pair of entities from (A, B) is associated with C, or (A, C) with B

# L4.5 ER3
## Top to bottom
### Method 1:
* Form a schema for higher level entity
* Form a schema for each lower level entity set include primary key o higher level entity
Drawback: Getting information about an employee requires accessing two relations,

### Method - 2:
* Form a schema for each entity set with all local and inherited attributes
Drawback: name, street and city stored redundantly

## Bottom up Design

### Completeness Constraint (Generalization/Specialization)
* Specifies whether or not an entity in the higher level entity set must belong to at least one of the lower level entity sets within a generalization
	* Total - should belong to lower
	* partial - needn't necessarily
* Partial Generalization - default
* total specified with keyword total and dashed line from keyword to hollow arrow head

## Aggregation
* Relationships sets represent overlapping information
	* for eg, some proj_guide relationships may not correspond to any eval_for relationships
	* cannot discard the proj_guide relationships
* Eliminate redundancy via aggregation without introducing redundancy
![[Pasted image 20250410183520.png]]
![[Pasted image 20250410183631.png]]

## Binary vs n-ary relationships
Althought possible to replace n-ary w several distinct binary, n-ary shows more clearly that several entities participate in a single relationship
Some better with binary:
* Eg parents, separate relation for mom and dad, helps with partial information
some better in non
* Eg: prof_guide

![[Pasted image 20250410184526.png]]

## Symbols:
![[Pasted image 20250410184725.png]]
