
# Hash Tables

## General principles 
- Use entire search key in the hash function 
- If the hash function uses modulo arithmetic, make table size of a prime number 
- simple and efficient hash is: 
	- convert to x, 
	- $h(x) = x \% \text{Table}_{size}$
		- Table size is the first prime number larger than twice the size of the number of expected values. 
	- determining a good hash function is a complex subject 
		- for now, we can just use provided hash functions

## Collision Handling 
- A collision occurs when two different keys are mapped to the same index 
	- Collisions may occur even when the hash function is good 
	- Inevitable due to pigeonhole principles (math220)
- There are two main ways of dealing with collisions 
	- Open addressing 
	- Separate chaining 

## Open Addressing 
- When an insertion results in a collision look for an empty array element 
	- start at the index to which the hash function mapped the inserted item. 
	- Look for free space in the array following a particular search pattern $\textemdash$ probing 
- There are three major open addressing schemes 
- Three major open addressing schemes 
	- Linear probing 
		- $(h(x) + p) \; \% \; \text{capacity}$
	- Quadratic Probing
		- $(h(x) + p^2) \; \% \; \text{capacity}$
	- Double hashing 
		- $(h(x) + ph_{2}(x)) \; \% \; \text{capacity}$

## Linear Probing
- The hash table is searched sequentially 
	- We start at the original hash location 
	- For each time the table is probed 
		- Search $h(\text{search key})$ =1, then $h(\text{search key}) +2$, and so on until an available location is found 
		- If the sequence of probes reaches the last element of the array, wrpa around to $arr[0]$
- Linear probing leads to primary clusterings 
	- The table contains groups of consecutively occupied locations.
	- These clusters tend to get larger as time goes on
		- Reducing the efficiency of the hash table.

## Linear probing example 
![[Pasted image 20260410002033.png]]
- assume we insert 35.
	- 35 mod 12 = 12. so we use linear probing to insert. 
	- We see that 12 + 1 is filled so we go to 12 + 2, then we insert at index 14. 
	- if we insert 60, we need to insert at index 15
	- if we add 12, we need to insert at at index 16 

## Searching 
- Searching for an item is similar to insertion 
- Ex. 
	- Find 59, mod 23 = index 12. 
	- if index 12 does not contain this, we use linear probing (or same collision resolution used for insertion) to find 59 or an empty space. 
	- If we get to an empty space, the element we are searching for is not in the table. 
- Search must use the same probe method as insertion 
- Terminates when item found, empty space, or entire table searched. 

## Hash Table Efficiency 
- When analyzing the efficiency of hashing, it necessary to consider a load factor, $\lambda$
	- $\lambda = \frac{n_{items}}{Size_{table}}$ 
	- As the table fills, $\lambda$ increases, and the chance of a collision also increases 
		- Performance decreases as lambda increases 
	- Unsuccessful searches make more comparisons 
		- Unsuccessful search only ends when free element is found 
- It is important to base table size on the large possible number of items 
	- The table size should be selected s.t $\lambda < 1/2$

## Double Hashing 
- in linear probing, the probe sequence is independent of the key. 
- Double hashing produces key dependent probe sequences 
	- In this scheme, a second hash function $h_{2}$, determines the probe sequence. 
- The second hash function must follow these guidelines 
	- $h_{2}(key) \neq 0$
	- $h_{2} \neq h_{1}$
	- A typical $h_{2} = p -( \text{key mod p})$

## Double hashing example 
![[Pasted image 20260410003402.png]]
- table is size 23. 
- hash function is h = x mod23, x is the search key value 
- the second hash function is 5 - (key mod 5)
- insert 81. 
	- 81 mod 23 = 12.
	- this collides with 58. 
		- use $h_{2}$ to find the probe sequence value 
	- $h_{2} = 5 - 81 \% 5 =4$. so we insert at 12 + 4 = 16. 
- insert 60, 60 mod 23 = 14. inserted.
- now, we insert 83.
	- 83 mod 23 = 14. collision with 60
	- $h_{2} = 5 - (83 \% 5) = 2$
	- first probe conflicts and then second puts us at 14 + 4 = 18

## Removals and Open Addressing 
- Removals add complexity to hash tables 
	- It is easy to find and remove a particular item 
	- Problem when we want to search for some other item 
	- The recently empty space may make probe sequence terminate prematurely 
- One solution is to mark a table location as either empty, occupied or removed (tombstone)
	- Locations in the removed state can be re-used as items are inserted 
		- only after confirming non-existence 

## Tombstones and Performances 
- After many removals, the table may become clogged with tombstones which must still be scanned as part of a cluster 
	- it may be beneficial to periodically re-hash all valid items 
- Example with linear probing h(x) = x mod 23. 
- the tombstone sentinel will result in continual searching. 

## Separate Chaining 
- Separate chaining takes a different approach to collisions 
- Each entry in the hash table is a pointer to a linked list (or any other dictionary compatible data structure)
	- If a collision occurs, the new item is added to the end of the list at the appropriate location 
- Performance degrades less rapidly using separate chaining 
	- with uniform random distribution, separate chaining maintains good performance, even at high load factors $\lambda >1$

## Separate chaining example. 
![[Pasted image 20260410011908.png]]
- insertion order: 81, 35, 60

## Hash Table Discussion 
- If $\lambda$ is less than $\frac{1}{2}$, open addressing and separate chaining give similar performance 
	- As $\lambda$ increases, separate chaining performs better than open addressing 
	- However, separate chaining increases storage overhead for the linked list pointers
- It is important to note that in the worst case hash table performance can be poor 
	- That is, if the hash function does not even distribute data across the table. 