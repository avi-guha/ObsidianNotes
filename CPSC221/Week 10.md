
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

# Priority Queues 
- solves the problem of finding an item with the highest priority. 

## Priority Queue ADT 
- A collection organized so as to allows fast access to and removal of the largest (or smallest) element 
	- Prioritization is a weaker condition than ordering 
	- Order of insertion is irrelevant 
	- Element with highest priority is first element to be removed 

## Priority Queue ADT 
- **Operations**
	- create 
	- destroy 
	- insert 
	- removeMin / removeMax 
	- isEmpty 
Priority Queue Property 
- For two elements x and y in the queue, if x has a higher priority value than y, x will be removed before y. 
- Note that in most definitions, a single priority queue structure supports only removeMin, or only removeMax, and not both. 

## Priority Queue Properties 
- A priority queue is an ADT that maintains a multiset of items 
	- A multiset allows duplicate entries 
- Two or more distinct items in a priority queue might have the same priority 
- If all items have same priority, does it behave like a FIFO?
	- depends on implementation 

## Priority Queue Applications 
- Hold jobs for a printer in order of size 
- Manage limited resources such as bandwidth on a transmission line from a network router 
- Sorting numbers 
- Anything greedy: an algorithm that makes the 'locally best choice' at each step

## Data structures for priority queues 
- Worst case complexities 


| **structure**      | **insert** | **removeMin** |
| ------------------ | ---------- | ------------- |
| unordered array    | O(1)       | O(n)          |
| ordered array      | O(n)       | O(n)          |
| unordered list     | O(1)       | O(n)          |
| ordered list       | O(n)       | O(1)          |
| binary search tree | O(n)       | O(n)          |
| AVL tree           | O(log n)   | O(log n)      |
| Binary heap        | O(log n)   | O(log n)      |
- heap has asymptotically same performance as AVL tree, but much similar to implement 

## Binary Heap 
- A heap is a binary tree with two properties 
- Heaps are complete 
	- All levels, except the bottom must be completely filled
	- The leaves on the bottom level are as far to the left as possible 
- Heaps are partially ordered 
	- For a max heap, the value of a node is at least as large as its children's values
	- For. a min heap, the value of a node is no greater than its children's values
- We need not be completely ordered ![[Pasted image 20260410103856.png]]

## Duplicate Priority Values 
- It is important to realize that two binary heaps can contain the same data, and some structure / shape but items may appear in different positions in the structure. 

## Heap Implementation 
- Heaps can be implemented using arrays 
- There are natural methods of indexing tree nodes 
	- Index nodes from top to bottom and left to right as shown
	- Since heaps are complete binary trees, there cannot be gaps in the array. 
Alternate implementation: 1-indexed 
- start at index 1 and index 0 is unused. 

## Referencing nodes
- To move around in the tree, it will be necessary to find the index of the parents of a node 
	- or children 
- The array is indexed from 0 to n-1 
- Each level's nodes are indexed from 
	- $2^{level} -1$ to $2^{level +1} -2$, root is at level 0 
	- The children of a node, i, are the array elements indexed at 2i+1 and 2i+2
	- The parent of a node i, is the array element indexed at (i-1) / 2(integer division)
- 1-indexed heaps use a slightly different calculation: 
	- children at 2i and 2i+1 
	- parent at i/2

**an array based heap will have no gaps, since the tree is complete**

## Heap Implementation 

```c;
template <class LIT>
class MinHeap {
private:
int size; // number of stored elements
int capacity; // maximum capacity of array
LIT* arr; // array in dynamic memory
public:
...
};

```

``` c; 
template <class LIT>
MinHeap::MinHeap(int initcapacity) {
size = 0;
capacity = initcapacity;
arr = new LIT[capacity];
}
```

## Heap Insertion 
- On insertion, the heap properties have to be maintained. 
	- A heap is a complete binary tree 
	- A partially ordered binary tree
- The insertion algorithm must first ensure that the tree is complete 
	- new item is the first available (right-most) leaf on the bottom level 
- Fix the partial ordering. 
- Compare new value to parent 
- Swap if new value greater than parent (max heap)Repeat until this is not the case 
	- this is heapify up

## Heap insertion complexity 
- Item is inserted at the bottom level in the first available space 
	- this can be tracked using the heap size attribute 
	- O(1) access using array index 
- Repeated heapify-up operations means 
	- each heapify-up operation move the inserted value up one level in the tree 
	- upper limit on the number of levels in a complete tree is O(log n)
- Heap insertion has worst case performance of O(log n)

## Building a heap 
- A heap can be constructed by repeatedly inserting items into an empty heap 
	- O(log n) per item 
	- O(n) items 
	- O (n log n) total

## Removing the priority item 
- Heap properties must be satisfied after removal 
- make a temporary copy of the root's data 
- Similarly to the insertion algorithm, first ensure that the heap remains complete 
	- Replace the root node with the rightmost leaf 
- Swap the new root with its largest valued child until the partially ordered property holds 
- return the copied root's data 

## Complexity of removeMin / RemoveMax 
- Analysis similar to insertion 
- replace root with last element 
	- O(1)
	- each heapify down moves one level closer to bottom of tree
- Removing the priority item from a heap is also O(log n) worst case


## Array implementation and insertion 
- Note that like other array-based structures, there are limited capacities at creation time 
	- We expand the array when full
- Since array indices correspond exactly to node positions in the tree, and node should remain in their original positions after expanding the array, we cans imply copy them into the same indices 

# Priority Queues 

## Creating heaps 
- To create a heap given a list of items: 
	- Create an empty heap 
	- For each item, insert into heap 
	- Complexity is O(log n)

## Build Heap
- Given the tree representation of some unordered array 
	- where can the heap variant initially be violated  
- Max hap invariant (partial order)
	- key value in every node must be larger than key value of children 
- To create a heap from an unordered array, repeatedly call heapifyDown 
	- Any subtree in a heap is itself a heap 
	- call heapifyDown on elements in upper 1/2 of array 
		- since lower half are leaf nodes and are already heaps 
	- start with index n/2 and work up to index 0 
		- from the last non-leaf node to the root 
	- heapifyDown does not need to be called on the lower half of the array 
		- since heapifyDown restores the partial ordering from any given node down to the leaves, the heap property remains satisfied at deeper levels of the tree. 

## buildHeap complexity 
- The cost for heapifyDown is O( height )
	- It would appear that buildHeap cost is O(n log n)
	- The cost is O(n)
- We can look at the total number of swaps that can occur in the worst case 
	- the bottom level is full, and contains all the highest priority values.

## $N_{swaps}$
- heapifyDown must follow some path along edges to the bottom of the tree 
	- we can count the total number of edges that can be followed in the worst case 
- At the next level, we still need to follow a single path.
![[Pasted image 20260410114306.png]]
- and also at the root level 
	- we have coloured the maximum number of edges that heapifyDown can use 
	- what is the upper bound on the number of edges in a tree with n nodes? 
	- exactly n-1 edges, thus worst case number swaps is O(n)

## Heapsort 
- Heapify the array
- Repeatedly remove the root 
	- After each removal swap the root with the last element in the tree 
	- the array is divident into a heap part and a sorted part 
- At the end of the sort the array will be sorted in ascending order 
- Complexity: $O(n) + O(n \log n) = O(n \log n)$

## nlogn sorting summary 


| **algorithm** | **best**  | **average** | **worst** | **space** |
| ------------- | --------- | ----------- | --------- | --------- |
| mergesort     | O(n logn) | O(n logn)   | O(n logn) | O(n)      |
| heapsort      | O(n logn) | O(nlog n)   | O(n logn) | O(1)      |

- heapsort can be implemented iteratively and requires no additional space except for local variables 