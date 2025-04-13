# L6.1 - Relational Database Design - Normal Forms
## Normalization/Schema REfinement
* organizing data in data
* Eliminates data redundancy and undersiable characteristics
	* insertion anomaly
	* update
	* deletion
## Desirable properties of decomposition
* Lossless join decompositoin
* Dependency preserving

## Normal forms
### 1st normal form
* atomic values only
Drawbacks: sometimes introduces redundancy, when lhs is not a superkey,

### 2nd Normal form
R in in 1nf and no partial dependency
![[Pasted image 20250412214333.png]]
#### Partial dependancy:
![[Pasted image 20250412214545.png]]

#### Key normalization
![[Pasted image 20250412214623.png]]

### 3rd Normal Form
In 2nf, does not contain transitive dependencies (no trivial dependencies)
(OR, every non-prime attribute of R is non-transitively dependent on every key of R)

#### Transitive dependency
functional dependency which hold by vitue of transitivity
Only occurs in relation that has three or more attributes
![[Pasted image 20250412215222.png]]
**Example**
![[Pasted image 20250412220447.png]]


# L6.2 - Normal form p2
# 3nf decomposition
Testing for 3nf is nphard
Decomposition into 3nf can be done in polynomial time

### algorithm
eliminate redundnat FDs, resulting in canonical cover Fc of F
Create a relation Ri