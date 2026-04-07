# Dictionary ADT

## Recursive traversal 
- pre order print: 
	``` c
	void PrePrint(Node *nd){
		if(nd != null){
		cout << nd -> data << " ";
		PrePrint(nd -> left);
		PrePrint(nd -> right);
		}
	}
	```

## Iterative Traversal 
- Explicit stack 
``` c; 
	void PrePrint(Node){
		Stack<Node*> st; 
		if( nd != null)
			st.push(nd);
			
			While(!st.IsEmpty());
				Node* curr = st.Pop();
				cout << curr -> data << " ";
				if (curr -> right != NULL)
					st.Push( curr -> right);
				if(curr -> left != NULL)
					st.push(curr -> left);
					
	}
```

- We can store more than just a Node pointer in the stack for complex tasks 
- Call stack is much more convenient 
- Since the longest path from root to any leaf is the tree's height, the space complexity is O(height)
- ![[Pasted image 20260307215736.png]]
- This structure has the max number of elements.

- We can also do this process with a queue 

``` c
Void PrePrint (Node* nd){
	Queue<Node*> q;
	
	if(nd != NULL){
		q.Enqeueue(nd);
	}
	
	while (!q.IsEmpty()){
	Node* curr = q.dequeue();
	cout << curr -> data << " ";
	
	if( curr -> left != NULL) 
		q.enqueue( curr -> left);
	}
	if( curr -> right != NULL){
		q.Enqeueue( curr -> right );
	}

}
```


## Motivation for an efficient lookup 
- Store values associated with user-specified keys 
	- Values may be any (homogenous) type 
	- Keys may be any (homogenous) comparable type 
- Dictionary operations 
	- create 
	- destroy 
	- insertfind 
	- remove 

![[Pasted image 20260307220821.png]]

- since values rarely affect our design choices, we can ignore them 

## Set ADT 
- Stores keys 
	- keys may be any homogenous comparable type 
	- Quickly tests for membership 
- Set operations 
	- Create 
	- Destroy 
	- insert 
	- Find 
	- Remove
### Data structures for Dictionary ADT
![[Pasted image 20260307220933.png]]


## Binary Search Tree Property 
- A binary search tree is a tree with a special property 
	- All nodes in the tree have: 
		- left subtree < label of the subtree's root 
		- nodes n the right have labels greater than that of the subtree's root 
	- BST is fully ordered

### BST inOrder traversal 
- if we traverse a BST inOrder we retrieve the data in the sorted order

## BST Search 
- To find a value in a BST search from the root node: 
	- If the target is less than the value in the node search its left subtree 
	- If the target is greater, search the right subtree 
	- return true if found 
- Comparisons 
	- one for each node on path 
	- worst case height of tree + 1 = $\lfloor \log(n) \rfloor$ +1 

# Reasoning about trees 

## Minimum waste 
- How much space does a (null-terminated singly) linked list waste when storing n values 
	- how much space is used for stuff that isn't actual data. 
- for example a null value wastes one pointer 

## Arrays
- how much space does an array waste when storing n values 
- if an array stores c values maximum, then we waste c-n space when we only store n out of those c values 

# Binary Trees 
- A binary tree wastes n+1 values 
- ![[Pasted image 20260307223648.png]]
- ![[Pasted image 20260307223702.png]]

### Proof: 
- in a binary tree with n values there are exactly n nodes 
- Exactly 1 of those nodes has no parent 
- other nodes have exactly one parent 
- every non-null pointer points from a parent to a child 
- there are exactly n-1 non-null pointers 
- in terms of n, total number of pointers in tree nodes is 2n 
- the number of nulls in a binary tree of n values in n+1 

## Number of nulls in a binary tree 
- Def: a finite binary tree is either an empty tree or non-empty tree ($v, T_{l}, T_{r}$), where v is a value and $T_{l}$ and $T_{r}$ are finite binary trees. 
- Theorem: A finite binary tree of n values has exactly n+1 nulls 
- Base case: n = 0, theorem states a finite binary tree of value has exactly 1 null. This is true. 
- Inductive case. Consider an arbitrary finite binary tree, where we let $n_{l}$ be the number of values in $T_{l}$ and $n_{r}$ be the number of values in $T_{r}$ 
- Inductive hypothesis: The number of nulls in $T_{L}$ is $n_{L}$+1 and $T_{R}$ is $n_R$ + 1
- Then, 
- T is constructed from 1 root node and T_l containing n_L node and T_R containing N_R nodes since T_l and T_R are strictly smaller trees and therefore covered by IH 
- Number of nulls in T = n+1

# Binary Search Trees 

## Binary Search Tree Property 
- Binary search tree is a binary tree with a special property 
	- For all nodes in the tree 
	- All nodes in the left subtree have labels less than the label of the subtree's root 
	- All nodes in a right subtree have labels greater than or equal to the label of the subtree's root 
- Binary search trees are fully ordered 

## BST Search 
- To find a value in a BST search from the root node: 
	- if the target is less than the value in the node search its left subtree
	- If the target is greater than the value in the node search its right subtree 
	- otherwise return true or etc. 
- Comparisons in terms of n 

## BST insertion 
- The BST property must hold after insertion as well 
- The new node must be inserted in the correct position 
	- This position is found by performing a search 
	- If the search ends at the null left child of a node make its left child refer to the new node 
	- If search ends at right child of a node make its right child refer to the new node 
- Cost is about the same as the cost for the search algorithm 
- ![[Pasted image 20260307225248.png]]
```c; 
void Insert(T key) {
    // initial recursive call
    root = InsertRec(root, key);
}

Node* InsertRec(Node* nd, T key) {
    // Base case: we've reached an empty spot, so create the new node here
    if (nd == nullptr) {
        return new Node(key);
    }

    // Recursive step: navigate left or right
    if (key < nd->data) {
        // The left child becomes the result of inserting into the left subtree
        nd->left = InsertRec(nd->left, key);
    } else if (key > nd->data) {
        // The right child becomes the result of inserting into the right subtree
        nd->right = InsertRec(nd->right, key);
    } 
    // If key == nd->data, it's a duplicate. We do nothing and just return 'nd'.

    // Return the root of the subtree after insertion
    return nd;
}

```

### Finding min and Max. 
- min: from root, follow left child until no more left children exist 
- max: from root, follow right child until no more right children exist.

## BST removal 
- After removal the BST property must hold 
- Removal is not as straight forward as search or insertion 
	- With insertion, the strategy is to insert a new leaf 
	- This avoids changing the internals structure of the tree 
	- This is not possible with removal 
		- Removed node's position not chosen by algorithm 
- Numerous cases to consider 

## BST Removal cases
- Node to be removed has no children 
	- Remove and asign null to parents reference 
- Node to be removed has one child 
	- Replace node with its subtree 
- Node to be removed has two children