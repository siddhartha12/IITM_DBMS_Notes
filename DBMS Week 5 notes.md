# L5.1 - Relational Database Design
## Good design:
* Reflects real world structure
* Can represent all expected data over time
* Avoids redundant storage of data items
* Provides efficient access to data
* Supports the maintenance of data integrity over time
* Clean, consistent and easy to understand

## Redundancy:
Having multiple copies of same data in the database
* this problem arises when a database is not normalized
* It leads to anomalies

## Anomaly:
inconsistencies that can arise due to data changes in a databse with insertiondeletion and update
occur in poorly planned un-normalised databases where all the data is storedin on table
three kinds:
* Insertion Anomaly:
	When insertion of data record is not possible without adding some additional unrelated data
	EG: cannot add instructor if department does not have building or a budget
* Deletion Anomaly:
	deletion of record results in losing some unrelated information that was stored as part of record
* Update Anomaly:
	Data is changed,  many records have to be changed, some go wrong

## Observations:
Redundancy => Anomaly

### Causes:
not independent columns, there are dependant columns
EG: department decides building and budget

### Removing and minimizing redundancy- Decomposition:
Decompose the relation into smaller relations
Good Decomposition => minimization of dependency

Not all decompositions are good
![[Pasted image 20250410225322.png]]

## Functional Dependency:
left hand side functionally decides the rhs
* Constraints on the set of legal relations
* Require that the value for a certain set of attributes determines uniquely the value for another set of attributes
* A functional dependency is a generalization of the notion of a key
Require that the value for a certain set of 
eg: department -> building, budget\
![[Pasted image 20250410232839.png]]
![[Pasted image 20250410233111.png]]
## Lossless Join decomposition
![[Pasted image 20250410230051.png]]
![[Pasted image 20250410230121.png]]

### Armstrong's axioms
![[Pasted image 20250410233643.png]]

## Atomic domains
### First Normal Form (1NF)
* A domain is atomic
	* eg of non atomic domains: Set of first and last name, identification number like CS101
* Relational schema R is 1NF if
	* domains of all attributes of R are atomic
	* Value of each attribute contains only a single value from that domain
* Non-atomic values complicate storage and encourage redundant storage of data

### Atomicity:
property of how the elements of the domain are used
* strings would normally be considered indivisible
* Suppose that students are given roll nos which are string of the from CS0012 and first 2 letters are extracted to find department, the domain si not atomic

For one-to-many relationships with functional dependencies, it's better to create 2 relations for 1NF

# L5.2 - Relational Database Design p2
* Decide whether a particular relation R is in good form
* In the case that a relation R is not in"good" form, decompose it into a set of relations {R1,---Rn} such that
	* each relation is in good form
	* Decomposition is a lossless join decomposition
* Theory based on:
	* Functional dependencies
	* multivalued
	* others

# L5.3 - Relational Database Design p3
![[Pasted image 20250410235247.png]]

## Armstrong's Axioms: Derived Rules
![[Pasted image 20250411000456.png]]

**Example**: This is called closure - Turns a dependency into a closure functional depency
![[Pasted image 20250411000417.png]]

## Closure of Attributes
* Testing for superkey:
	* To test if A is a superkey, we compute A+ and check if A+ contains all attributes of R
* Testing Functional Dependencies
	* To check if a functional dependency A -> B holds (or, in other words, is in F+), just check if B subset of A+
	* We compute A+ using attribute closure, then check it if contains B
* Computing closure of F
	* For each G subset  ofR, we find G+. and for each S subset of G+, we output a functional dependency G -> S

## Boyce-Codd Normal Form
A relation schema R is in BCNF with respect to a set F of FDs if for all FDs in F+ of the form
A -> B, where A subset of R and B subset of R, at least one of the following holds:
* A -> B is trivial 
* A is a superkey for R
### Decomposition
![[Pasted image 20250411002153.png]]

### Lossless Join
![[Pasted image 20250411002944.png]]

### Dependency Preservation - 3NF
![[Pasted image 20250411003210.png]]

## Third Normal Form
![[Pasted image 20250411003357.png]]

# L5.4 - Relational DAtabase Design p4
![[Pasted image 20250411005445.png]]
![[Pasted image 20250411005645.png]]

## Equivalence of sets of functional dependencies
* F and G are funcitonal dependencies
	* two sets F and are equivalent is F+ = G+
	* Equivalece means that every functional dependency in f can be infered from G, vice cersa
	* ![[Pasted image 20250412211955.png]]

## Canonical Cover
* sets of FDs may have redundant dependencies can be infered from the others
* Canonical cover for F is a set of dependencies Fc such that all the following properties are satisfied
	* F+ = Fc+
	* No functional dependency in Fc contains an extraneous attribute
	* Each left side of funcitonal dependency in Fc is unique. That is there are no two dependencies ![[Pasted image 20250412212206.png]]
* **Example**
* ![[Pasted image 20250412212245.png]]
* ![[Pasted image 20250412212312.png]]
### Computing Canonical cover![[Pasted image 20250412212406.png]]

## Prime and Non-Prime attributes
![[Pasted image 20250412212539.png]]


# L5.5 p5
## Lossless Join Decomposition
![[Pasted image 20250412212645.png]]

## Dependency Preservation
![[Pasted image 20250412212813.png]]

### Testing
![[Pasted image 20250412212919.png]]

![[Pasted image 20250412213038.png]]