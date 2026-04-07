
# BST Removal 

## Preview  
- After removal, BST property must hold 
- Removal is not as straight forward as search or insertion 
	- with insertion strategy is to insert a new leaf 
	- This avoids changing internal structure 
	- Not possible with removal
		- removed node is not chosen by algorithm 


## Looking at the next node 
- One of the issues with implementing a BST is the necessity to look at both children 
	- just like a linked list, we nee to look ahead for insertion and removal 
	- check that the node is null before accessing its member variables 
- Consider removing a node with one child 

## Looking Ahead 
- step 1: we need to find the node to remove and its parent 
- To make the correct link, we need to know if the node to be removed is a left or right child. 

``` c;
while (nd != NULL && nd -> data != target){
	if (target < nd -> data){
		parent = nd; 
		nd = nd -> left 
		isLeftChild = true; 
	} else {
		parent = nd; 
		nd = nd -> right; 
		isLeftChild = false; 
		}
	}
```

## Removing a node with 2 children 
- This is the most difficult case 
	- The strategy when the removed node had one child is to replace it with its child 
	- When a node has two children we have problems. 
- We need to figure out which child we should replace the node with 
	- could solve by just picking one 
- What if node we replace it with also has two children 
	- Causes issues. 

## Finding the Predecessor 
- When a node has two children, instead of replacing it with one of its children, we find its in-order predecessor 
	- A node's predecessor is the rightmost node of its left subtree. 
- The predecessor cannot have a right child and can therefore have at most one child 

## Why use the predecessor? 
- The predecessor has the following useful properties 
	- Because of BST property it must be the largest value less than its ancestor's value 
		- It is less than the nodes in its ancestor's right subtree 
		- it is to the right of all nodes in ancestor's left subtree so it must be the largest. 
	- These properties make it the best candidate to replace its ancestor. 

## The successor 
- The in-order successor to a node is the leftmost child of its right subtree 
	- It has the smallest value greater than its ancestor's value 
	- it cannot have a left child 
- The successor can also be used to replace a removed node 
	- Pick either one, but must be consistent. 


## Time Complexity 
- Total cost for removal 
	- search for target is O(height)
	- If 2 children, find predecessor or successor 
		- O(height)
	- 4 pointer assignments 
		- O(1)
	- O(height)

## Set/Dictionary Implementation 
- Worst case complexity for important set/dictionary operations 
- BST 
	- Search = O(n)
	- Insert = O(n)
	- Remove = O(n)
- Looks bad, as bad as using a sorted linked list 
- does better on average but we can guarantee doing better 

## Balanced Binary trees 
- A binary tree is balanced if 
	- Leaves are all about same distance from the root 
- Sometimes trees are balanced by comparing height of nodes 
	- Height of a node's right subtree is at most one different from the height of its left subtree 
- Sometimes a tree's height is compared to number of nodes 
	- length of longest path $\leq$ 2 (length of shortest path)
![[Pasted image 20260308121518.png]]



## Problem solving BST 
- Removal, don't  be retarded 
- searching for a value 
	- think about ranges 
		- left step lowers ceiling 
		- right step raises the floor
- Building binary trees, don't be retarded 

## AVL Trees 
- An AVL tree is a balanced BST 
	- Each node's left and right subtree differ in height by at most 1 
	- Rebalancing via rotations occurs when an insertion or removal causes excessive height difference 
- AVL tree nodes contain extra info to support this height information 
- ![[Pasted image 20260314190801.png]]

## Balanced Binary Trees 
- By AVL property L/R subtree heights differ by at most 1 
![[Pasted image 20260314191551.png]]

## Imbalanced binary trees 
- By AVL property - L/R subtree heights differ by at most 1 

## AVL Tree height 
- Height of an AVL tree containing n key values is O(log(n))
- Intuition: 
	- For a fixed height h, a tree containing fewer nodes has a larger height-to-node ratio 
- Attempt to achieve the worst ratio by making an AVL tree of height h with the minimum number of nodes. 
- Theorem: the height of an AVL tree with n nodes is O(log(n))
- Proof: 
	- Let $N_{h}$ represent the minimum number of nodes in an AVL tree of height h
	- Since the AVL property must be satisfied at every node, the children of such a tree must also be minimal 
	- We further minimize the size by maximizing the height difference between the children 
	- Thus, $N_{h} = 1+ N_{h-1} + N_{h-2}$

## Rotations 
- An item must be inserted into a BST at the correct position 
- The shape of a tree is determined by 
	- The value of the items inserted into the tree 
	- The order in which those values are inserted 
- This suggests that there is more than one tree that can contain the same values 
- A tree's shape can be altered by rotation while still preserving the BST property 
	- The inOrder traversal is also preserved 

![[Pasted image 20260314223214.png]]
![[Pasted image 20260314223223.png]]

## Left Rotation example: 
- Create pointer to X's right child 
- Set X's right child to temp's left child 
- Detach temp's left child
- Make x the left child of temp 
- Make temp the left child of x's parent

# AVL trees 
- An AVL tree is a balanced BST 
	- Each node's left and right subtrees differ in height by at most 1.
	- Rebalancing via rotations occurs when an insertion or removal causes excessive height difference.
## AVL Nodes 
- AVL node is almost the same as a binary tree node 
- Additional balance field indicates that state of subtree balance at that node 
- there are alternate implementations 

![[Pasted image 20260314230027.png]]

### 4 types of Imbalances 
- Left - Left 
- Left - Right 
- Right - Right 
- Right - Left
**Runtimes**
- Predecessor/Successor: O(n)
- Build BST O(n^2)
- Compute height of every tree O(n)
- Determine if tree is BST O(n)
- Inserting a BST node between 1 and n
- Search for a BST key between 1 and n 
- Find min/max key in BT is O(n)
- almost everything except building is O(n)

## AVL Insertion 
- The best way to keep a tree balanced, is to never let it become imbalanced 
- AVL insertion begins with regular BST insertion followed by rotations to maintain balance 
	- AVL properties must be satisfied before and after insertion 
	- if the balance attribute of a subtree's root node becomes critical as a result of inserting the new leaf then rebalance it
- ![[Pasted image 20260314231615.png]]
- ![[Pasted image 20260314231712.png]]
- Cost of AVL insertion is O(logn)

### AVL removal 
- AVL properties must be satisfied before and after removal 
- Begin with basic BST removal 
	- Local root balance is adjusted for removing from left or right subtree 
	- maintain a decrease variable to indicate whether the height of the subtree decreased 
	- Use decrease variable to increment or decrement local root balance 
	- rebalance if critical. 

## AVL tree removal example 3 
- BST removal O(height) $\in$ O(log(n))
- backtrack, pay O(1) per critical imbalance 
- Up to log(n) critical imbalances 