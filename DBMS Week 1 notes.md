making my first note after [[Welcome]] and after browsing through [[create a link]]

# Summary:

# L1.2 - Why DBMS p1
Data Management is need for civilized society:
* storage 
* Retrieval
* Transaction
* Audit (to make sure everything has gone well)
* Archival (for some possible future reference)


#### Things started streamlining towards the 20th century
Before the 20th Century Ledgers were used and the process was known as bookkeeping
in 1886, Henry Brown, an American Inventor patented a receptacle for storing and preserving papers
Herman Hollerith adapted punch cards for weaving looms, mechanical computation

#### Middle of 20th Century Advances in Technology

Punch cards, magnetic tape was initial data storage types
Later, digital  came in 


#### Problems with bookkeeping

Durability
Scalability
Security
Retrieval
Consistency 

* Spreadsheets have better of all the above, Mostly useful for single user or small enterprise applications

## Why Modern DBMS

Drawbacks of Spreadsheet
* Limited rows
* Consistency errors
* No means  to check violations of constraints in the face of concurrent processing
* Unable to give different permissions to different people in a centralized manner


# L1.3 - Why DBMS p2

sql has ability to rollback etc.

## Comparison of DBMS over Python File Handling
![[Pasted image 20250118193355.png]]

### Scalability comparison
![[Pasted image 20250118193628.png]]

### Time and Efficiency Comparison
![[Pasted image 20250118193701.png]]

### Persistence, Robustness and Security Comparison
![[Pasted image 20250118193736.png]]

### Productivity
![[Pasted image 20250118193805.png]]

### Arithmetic Operations
![[Pasted image 20250118193835.png]]

### Cost and Complexity
![[Pasted image 20250118193900.png]]


# L1.4 - Intro to DBMS - 1

## Levels of Abstraction
* **Physical Level**: Describes how record is stores
* **Logical Level**: Describes how data stores in database and relationships among the data fields
* **View Level**: Application level programs hide details of data types. (Views can hide information, eg: employee salary)

Physical and Logical level is back end ID team. View level is for application purposes. View level has multiple instances.

## Schema and Instance

### Schema: 
* Way the data is organized
* Divided into
	* Logical Schema: Overall logical structure of a database
	* Physical Schema: Overall physical structure of the database
### Instance:
is an occurrence of a data type
Actual content of a database at the particular point in time

## Physical Data Independence:
**Physical Data Independence** in a **Database Management System (DBMS)** refers to the ability to modify the physical storage structure or access methods of data without requiring changes to the logical structure or application programs that use the data. This ensures that changes to the underlying hardware or data storage mechanisms do not disrupt the database's higher-level schema or functionality.

## Data Models

A collection of tools for describing:
* Data
* Data Relationships
* Data semantics
* Data constraints

Relational Model - focused on this course 

## DDL - Data Definition Language

DDL - The Specific notation for defining the database schema
DDL compiler generates set of table templates stored in a Data Dictionary
Data dictionary contains metadata
* Database Schema
* Integrity constraints
* Authorization

## Data Manipulation Language

Language for accessing and manipulating data organized by the appropriate data model
* **AKA Query Language**
Two classes of language
* Pure - Used for proving properties about computational power and optimization
	* Relational Algebra
* Commercial
	* SQL
## SQL
Not a Turing Complete Language
Most popular
To be able to compute complex function, SQL embedded in some higher level language

# L1.5 - Intro to DBMS - 2

## Database Design
Logical Design:
* Deciding on database schema.
* Business decision (What attributed to be recorded)
* Computer science decision (What relational schema used and what attributes distributed where)
Physical Design:
* Deciding on the physical layout of the database

## Design Approaches

Need to come up with a methodology to ensure that each relations in the database is good.
Two ways of doing:
* Entity Relation Model
* Normalization Theory

## Object Relational Data Models

Relational model has flat atomic values
Object relational model extends relational model by including object orientation and constructs to deal with added data type
(Kind of adds new objects and classes like OOP to data types)

## Database Engine
Has 3 major parts
* Storage Manager
* Query Processing
* Transaction Management
### 1. Storage Manager
Program module that provides the interface between the low-level data stored in the database and the application programs and queries submitted to the system.

Responsible for:
* Interaction w/ OS File Manager
* Efficient storing, retrieving and updating of data

### 2. Query Processing

![[Pasted image 20250118213505.png]]

Alternative ways of evaluating a given query
* Equivalent expressions
* Different algorithms for each operation
Cost difference between a good and bad way of evaluating a query can be enormous.

### 3. Transaction Management
(Every action needs to be serialized)
When more than one user is concurrently updating the same data. A transaction is a collection of operations that performs a single logical function in a database application. 
* **Transaction Management component** ensures that the database remains in a consistent state despite system failures.
* **Concurrency-control Manager** controls the interaction among the concurrent transactions, to ensure the consistency of the database
