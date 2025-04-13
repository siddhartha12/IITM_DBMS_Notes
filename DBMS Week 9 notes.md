# L 9.1 - Indexing

## Search records
has key and a pointer - for each column in table

Two kinds of indices:
* ordered indices: search keys are stored in sorted order
* hash indices: search keys are distributed uniformly across buckets using hash function
![[Pasted image 20250413023741.png]]

## Ordered indices
search key value
primary index - sequentially ordered
	clustering index
secondary index: whose searhc key specified an order different from the sequential order of the file
* also called non-clustering index
dense index: all rows primary key is stored
sparse index: only a few and in order

![[Pasted image 20250413024105.png]]
indices offer benefir while searching


## Multilevel index
outer index; sparse inddex of primary index
inned index: primary index

## deletion
single level index -> simple index is deleted
sparse -> just think