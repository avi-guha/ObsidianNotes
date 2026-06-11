---
~
---
**Prim's Algorithm:**
- minimum spanning tree 
	- there can only be multiple different MST if there is a tie between edge weights that forces the algorithm to make an arbitrary choice 
	- uniqueness occurs if the minimum travelling between two edges is less than anything else 
	- you cannot go back to a visited vertex with Prim's algorithm 
	- all edges attached to our spanning tree at any given step are valid edges
	- sorted array - O(mlog(n)) , where m is the number of edges and n is the number of vertices 
	- unsorted is O(m^2)
- Start at A 
		- A-B 
		- A-B-C
		- A-B-C-D
		- A-B-C-D-E
		- A-B-C-D-E-F
	- ![[Pasted image 20260418115632.png]]
- maximum spanning tree is the opposite of minimum spanning tree.
- requires a priority queue and a graph.
**Kruskal's Algorithm**
- Greedy algorithm for connected Graph
- finds the cheapest available edges across an entire graph regardless of where they are
- Steps 
	- Sort 
		- sort edges in ascending order from lowest to highest weight
	- Select 
		- start with lowest weight edge and evaluate one by one
	- Check for cycles 
		- if adding edge that connects two vertices that are not already connected to ecah other (directly or longer path) add it to MST, add it. If they are already connected, we create a loop so do not add it.
	- unsorted array means we need to search through m elements m times 
**Djikstra's Algorithm**
- uses a priority queue and a graph
- used for shortest distance. NOT for MST.
**BTrees in Memory**
- B-trees are sorted pieces of data
- this means that i and i+1 will be very close together, but i and j with j >> 1 will not be 

**Hash Tables**
- do not require comparable keys 

**Dictionaries**
- require fast arbitrary lookups

**Disjoint sets**
- can be used to show an equivalence relation
**heap**
- these are already partially sorted 

**Shortest Paths**
![[Pasted image 20260418154343.png]]
- shortest path is 13 + x + 4 + 3 = 20 + x
- other paths: 
	- ACE: 15
	- ACDE: 13 
- 20 + x < 13 
- $x < -7 \implies x \leq -8$


![[Pasted image 20260418162413.png]]
- correct answer is -12. 
- a shortest path will always exist unless there is a negative weight cycle along the way.. 

**Disjoint sets**
![[Pasted image 20260418233144.png]]
- mlog*(m) is iterated log. disjoint sets have this property
- best case for union is O(1)

## General strategies for pointer-heavy problems 
- When dealing with nodes, links and structural modifications 
	- always draw out the problem 
	- cache before you overwrite. 
	- prevents the issue of overwriting a pointer 
	- check your traversals to ensure we are solving properly 

## Recursive problems 
- first establish the base cases 
	- most trivial case of the problem 
- Assume recursion works 
- current node logic 
	- if recursive calls handle the children, we need to worry about what the current node needs to do 
- key ideas: 
	- pass by reference 
		- we are getting actual pointer inside the node 
	- returning new root 
		- must ensure we return the new root 
	- update metadata manually
		- ensuring we update height etc. 
	- short-circuit 
		- ensure that we have fail-fast logic 