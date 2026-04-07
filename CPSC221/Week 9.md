
## B-trees
- Properties search 

## AVL trees are great? 
- Observations: 
	- AVL balance invariant guarantees worst-case O(log(n)) time for find, insert, remove 
	- Remember that nodes are allocated one at a time in dynamic memory. 
		- NO guarantee that parents/children, siblings are near to each other in memory. 
- Realities 
	- For large date sets, disk accesses dominate runtime. 

## A big BST 
- Suppose we have a very large data set stored in a BST 
	- So large that we cannot fit the entire tree inside of RAM 
	- Then the tree must reside on disk 
- Different levels of memory are accessed in blocks 
	- For disk memory, the block unit is called a page. 
	- BST nodes may all reside in different blocks 
	- a tree operation involving several nodes may require many expensive disk seeks 
- Goal: put more relevant data together in a single block so they can be retrieved in a single disk access. 
	- increase the number of keys in a single node 
	- minimized disk seeks 

## B-Tree nodes 
- A binary search tree node has 1 key and up to 2 children 
	- makes 2-ary tree 
- A B-tree node defines an m-ary tree 
	- each node has up to m-1 keys and up to m children 
	- nodes are still fully ordered 
	- additional pointers: parent, left sibling, right sibling. 
- Ideally, maximize the size of a B-tree node to fill a disk block. 
	- >100 are used. 

## B-tree properties 
- B-tree order m is an ordered m-ary tree 
	- internal node: \#keys = \#children -1
	- All leaves are at the same depth, all root to lead paths have the same length 
	- all leaves hold no more than m-1 keys 
	- all non-root internal nodes have between $\lceil m/2 \rceil$ and m children 
	- root can be a leaf or have between 2 and m children. 

## Search 
![[Pasted image 20260314235514.png]]


## Search Complexity 
- For each node, we must perform a linear search through m-1 keys 
	- O(m) per node
- How many nodes do we need to search 
	- O(height)
- Then B-tree search is O($m \cdot height$)
- What is the worst height of a B-tree containing n keys. 
	- Try to maximize height, and minimize the number of keys 
	- Minimizing keys leads to minimizing the number of nodes, except for the root level. 
	- ![[Pasted image 20260314235757.png]]

## Search Complexity 
- For each node, we must perform a linear search through m-1 keys 
	- O(m) per node, typically m is a constant 
- How many nodes do we need to search 
	- O(height)
- Then B-tree search O($m \cdot height$)
- What is the worst height of a B-tree containing n keys 
	- Try to maximize height, and minimize the number of keys 
	- Minimizing keys leads to minimizing the number of nodes, except for the root level. 
- For a tree of height h, the min number of nodes that we can have that will satisfy the b-tree properties. 
- Letting t = m/2. The total number of nodes x is: 
	- $x = 1 + 2 \sum^{h-1}_{i=0}t^i$
	- ![[Pasted image 20260315104918.png]]
	- height is less than O($\log_{m}n$)

## B-tree insertion 
- like BST search for insertion location 
	- Insertion always starts at leaf node 
- otherwise plot the node and send median upwards

# Introduction to Hash Tables 

## Dictionary ADT 
- Stores values associated with user-specified keys 
	- Values may be any homogenous type 
	- keys may be any homogenous comparable type
- Dictionary operations 
	- Create 
	- Destroy 
	- Insert 
	- Find 
	- Remove
# Implementing a dictionary 
- worst case complexities for Insert, Remove, Find 
- ![[Pasted image 20260315111957.png]]


# Problem Solving 

**AVL Trees**
AVL trees must differ in height by at most 1 
Min number of nodes is 2N-1 
Max nodes = $2^{h+1} - 1$

**Time Complexity**
Worst case of find is O(log n)
Worst case remove is O(log n)
Worst case insert is O(log n)
Worst case rotate is O(1)

**AVL Rotations**
For LL Tree - Right rotation 
For LR Tree - Left rotation then Right notation 
For RL Tree - Right rotation then Left Rotation 
For RR Tree - Left rotation

**Adding for Rotations**
- To cause a single right (LL)
	- need to make tree heavy in straight line to the left 
- To cause single left (RR)
	- need to make tree heavy in straight line to right 
- To cause LR rotation (LR)
	- kink that goes left then right 
- To cause RL rotation (RL)
	- kink that goes right then left