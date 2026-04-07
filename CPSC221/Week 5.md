
# Sort Complexity 
## Lower Bounds of Sorting 

### Comparison Based Sorting 
- Suppose we have an array of length n, which we ask our algorithm to sort. 
	- The algorithm performs a particular sequence of operations to transform the input permutation into the ordered permutation 
		- if given different input permutation, algorithm performs different sequence of operations to produce ordered permutation 
- Correct algorithm must be able to transform every/any input permutation into the ordered permutation 
	- The specific sequence of actions is determined by the result of successive comparisons between two elements a and b 
	- The algorithm can be described as a binary decision tree. 

### Lower bounds on sorting 
- ![[Pasted image 20260212235446.png]]
- We ask, how many leaves are in the decision tree 
	- How many starting permutations are there? 
		- An algorithm must be able to correctly sort every permutation 
	- n! input permutations, therefore n! different paths to a leaf. 
		- if there were less than n! paths, then an algorithm is not able to do some paths correctly. 
	- What is the minimum height of a tree with n! leaves?
		- A perfect tree with n! leaves.
- A perfect tree with h levels has $2^{h+1} - 1$ leaves. 
	- Bottom level has $2^h$ nodes (leaves), $2^{h}= n!$
	- $\log(2^{h)} = \log(n!)$
- ![[Pasted image 20260212235819.png]]

### Getting Recurrences 
- $T(X) \leq Y$
	- Base case size (usually for some specified constant)
	- Base case cost (usually for some unspecified constant)
- $T(N) \leq AT(X) + B$
	- A tells us how many sub problems we break into 
	- X tells us the size of each subproblem, X < N 
	- B Tells us the cost of non-recursive operations 

### Binary Search 
- Inspect each midpoint, search left or right half of array 

### Solving exact recurrences 
- Same techniques apply but without using inequality 

# Trees 


### Review: linked lists 
- Linked lists are constructed out of nodes, consisting of: 
	- a data element 
	- a pointer to another node 
- Lists are constructed as chains of such nodes 

### Trees 
- Trees are also constructed from nodes 
	- Nodes may now have pointers to one or more other nodes 
- A set of nodes with a signle starting point 
	- This is called the root of the tree 
- Each node is connected by an edge to another node 
- A tree is a connected graph 
	- There is a path to every node in the tree 
	- A tree has one less edge than the number of nodes 

![[Pasted image 20260213000703.png]]

### Tree terminology 
- A leaf is a node with no children 
- A path is a sequence of nodes $v_{1} .. v_{n}$
	- v is a parent of $v_{i+1} (1 \leq i \leq n)$
- A subtree is any node in the tree along with all of its descendants 
- A binary tree is a tree with at most two children per node 
	- The children are referred to as left and right 
	- We can also refer to left and right subtrees 

**Example**
![[Pasted image 20260213001248.png]]

![[Pasted image 20260213001313.png]]


## Measuring Trees 
- The height of a node v is the length of the longest path from v to a leaf 
	- The height of a tree is the height of the root 
- The depth of a node v is the length of the path from v to the root 
	- This is also referred to as the level of a node 
- There is a slight different formulation of the height of a tree. 
	- Where the height of a tree is said to be the number of different levels of nodes in the tree 

**Example**
![[Pasted image 20260213001610.png]]

**Perfect Binary Trees**
- A binary tree is perfect if 
	- No node has only one child 
	- All the leaves have the same depth 
- A perfect binary tree of height h has 
	- $2^{h+1} -1$ nodes, of which $2^h$ are leaves 
	- Perfect trees are also complete

### Height of a perfect tree 
- each level doubles the number of nodes 
	- level 1 has 2 
	- level 2 has 4 
- Tree with h levels has $2^{h+1}$ -1 nodes 
	- The root level has 1 node. 

### Complete Binary Trees
- A binary tree is complete if 
	- The leaves are on at most two different levels 
	- The second to bottom level is completely filled 
	- The leaves on the bottom level are as far to the left as possible 

### Full Binary Trees 
- A binary tree is full if every node has exactly 0 or 2 children 

# Binary Tree Traversal 

### Binary Tree Traversal 
- A traversal algorithm for a binary tree visits each node in the tree 
	- Typically it will do something while visiting each node 
- Traversal algorithms are naturally recursive 
- There are three traversal methods 
	- inOrder 
		- recursion(left)
		- current 
		- recursion(right)
	- preOrder
		- current 
		- recursion(left)
		- recursion(right)
	- postOrder 
		- recursion(left)
		- recursion(right)
		- current 

 **inOrder Traversal** 
```c;
 void inOrder(Node * nd){
	 if(nd!= nullptr){
		 inOrder(nd -> leftchild);
		 visit(nd);
		 inOrder(nd -> rightchild);
	 }
 }
```

**preOrder traversal** 
```c;
 visit(nd);
 preOrder(nd -> leftchild);
 preOrder(nd -> rightchild);
```

**postOrder traversal**
``` c;
 postOrder(nd -> leftchild);
 postOrder(nd -> rightchild);
 visit(nd);
```

### What is 'visiting'
- Same operation to be done at the current node 
	- counting or arithmetic 
	- creating a node 
	- deleting a node 
- height 

**Quiz**
Complete: 
- perfect for row above  (left to right)

Full: 
- 0 or 2 children 
- At min 
- At most perfect tree 

Perfect 
- Triangle 

**Problem solving**
- height of complete: closest power of two that does not go over
- Max nodes of complete is: $2^{h+1} -1$
- Max nodes of perfect tree: $2^{h+1} -1$ 
- Min nodes of a tree is $h+1$
- Number of leaves is $2^h$
- height of any tree is $\lfloor \log_{2}n \rfloor$
- 