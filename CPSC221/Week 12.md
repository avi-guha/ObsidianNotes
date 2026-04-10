
# Graph Traversal 

## Handshaking theorem
$\sum^{v \in V}_{0}deg(v) = 2|E|$
- degree is how many connections come out of. vertex 
- sub graph x #subgraph = total vertice 

## Graph traversal 
- Objective: visit every vertex and every edge in the graph, respecting the graph's structure 
- Purpose: we can search for interesting substructures in the graph, like solution paths in a game tree, shortest path to enemy etc. 
- We can contrast graph traversal to BST traversal: 
	- we need to figure out where to start 
	- which order to visit the neighbours 
	- cycles may exist

## Breadth-first search 
- Visits all vertices within d "hops" away from starting vertex, before visiting vertices within d+1 hops, where d begins at 0. 
![[Pasted image 20260410115801.png]]

- There are two parts of BFS 
	- Traverse_BFS(G) - the outer wrapper that handles the whole graph: 
		- Initializes all vertices as unvisited and all edges as unexplored 
		- then launches BFS(G,v) from every unvisited vertex 
	- BFS(G,v) - one BFS from a single starting vertex
		- uses a queue to process vertices level by level 
		- for each neighbor w of the current vertex: 
			- unvisited w -> edge is discovery edge, w gets enqueued 
			- w already visited -> edge is cross edge 
	- example 
		- Discovery edges form a BFS spanning tree / forest and give the shortest path from the start to every vertex 
		- Distances represent the unweighted shortest path length 
		- one BFS call visits exactly one connected component 
	- Time Complexity 
		- while loop: each vertex is enqeued/dequed once $\Theta(n)$
		- For loop: each vertex's neighbors are itereated -> total work is $\Sigma (dev) = \Theta(m)$
		- Initialization: $\Theta(n +m)$
	- $\Theta(n) + \Theta(m) +\Theta(m) = \Theta(n+m)$

# Graph traversal 
## Depth First Search (DFS)
- visits vertices along a single path as far as it can go, and then backtracks to the first junction and resumes down another path

## DGS Algorithm 
- Algorithm: mirrors BFS structure, but uses recursion instead of queue 
- **Traverse_DFS(G)** - outer wrapper
	- initializes all vertices as unvisited and all edges as unexplored 
	- launches DFS(G,v) from every unvisited vertex (handles disconnected graphs)
- DFS(G,v) - one recursive DFS from vertex, v: 
	- For each neighbor w: 
		- if w is unvisited -> edge(v,w) is a discovery edge -> recurse into w
		- if edge (v,w) is still unexplored -> it's a back edge (leads to previously visited ancestor)
	- Key difference from BFS: back edges go to already-visited vertices, whereas BFS has cross edges 
- Example walkthrough 
	- Running DFS(G,A) on a 5-node graph. The adjacency list table shows the order neighbours processed, with visited checked off
		- Discover edges form a DFS spanning trees 
		- Support pre - and - post order traversals 
		- one DFS(G,v) visits exactly one connected component.
		- Has useful applications like articulation point (bridge) discovery 
- time complexity 
	- Initialization: $\Theta(n) + \Theta(m)$
	- DFS called at most once per vertex (since vertices get marked visisted)
	- For loop per vertex: iterates over deg(v) neighbors → total Σ deg(v) = Θ(m)
- Total adjacency list: $\Theta(n+m)$
- with adjacency matrix it is $\Theta(n^2)$

# Example graph problems 
- topological sort 
- maze construction 
- spanning tree

## Partial Order 
- output a sequence of vertices such that no vertex $v_{j}$ has a directed edge to $v_{i}$, where $v_{i}$ is output before $v_{j}$

## Topological sort 
- Given a graph G = (V,E), output all vertices in V such that no vertex is output before any other vertex with an edge to it 
- Classic example, planning course schedule with prerequisites 
- compilers: 
	- determine a compatible order for compiling independent modules 

## Topological Sort 
- Label each vertex's in-degree (# inbound edges)
- Initialize a queue to contain all vertices with in-degree zero 
- While there are vertices remaining in the queue 
	- Pick a vertex v from the queue and output it 
	- Reduce the in-degree of all vertices adjacent to v 
	- Put any of these with updated zero in-degree on the queue 
	- Remove v from the queue. 

## How are algorithms affected? 
- Label each vertex's in-degree 
- initialize a queue to contain all vertices with in-degree zero 
- While there are vertices remaining in the queue 
	- Pick a vertex v from the queue and output it 
	- Reduce the in-degree of all vertices adjacent to v 
	- Put any of these with updated zero in-degree on the queue 
	- Remove the v from the queue 

## Maze Construction Problem 
- A bunch of adjacent rooms 
	- Each room is a vertex 
	- Open wall between rooms form edge 
	- Unpredictable, not easily solved 
	- highly branching, many dead ends 
	- just enough walls to get from any room to any other room 
		- especially start and finish 
- Given 
	- collection of rooms V, with cardinality >= 1
	- connections between rooms, E
- Construct a maze 
	- collection of rooms: V' = V
	-  designated rooms "in", i iin V and out o in V 
	- collection of connections to knock down E' subset E such that one unique path connects every pair of rooms 

## Maze under construction 
- So far, walls have been knocked while other remove 
- Now we consider walls between rooms 
	- if A and B we knock 
	- if not otherwise connected do not knock 
- Algorithm:
	- While edges remain in E 
		- Remove a random edge e = (u,v) from E 
		- If u and v have not been connected 
			- Add e to E' 
			- Mark u and v as connected 

## Spanning tree 
- Spanning tree: a subset of edges form a connected graph that 
	- touches all vertices in the graph (spans the graph)
	- forms a tree (is connected and contains no cycles)
![[Pasted image 20260410130230.png]]
- we already know two ways of constructing spanning tree - BFS and DFS 
	- discovery edges form BFS/DFS spanning tree 
	- produced for free from a traversal, so cost is the same as performing the traversal 
- In weighted graphs: minimum spanning tree 
	- the spanning tree with the least total edge cost 

## Problem solving 
![[Pasted image 20260410130602.png]]
- connected means there is a path between every pair of nodes 
	- if one node is isolated, then it is unconnected 
- Simple means there is at most edge per pair, if there are multiple it means we have a multigraph 
- Undirected graphs mean that edges are symmetric, A->B traversal means we can go B->A
- Max edge = n(n-1) /2, with n vertices (greatest lower bound)
- Edge = n-1 with n vertices (lowest upper bound)
- complete means that every vertex is connected to every other vertex
- no cycle means that DFS produces no back edges 
- a connecting graph is only a tree if it has n-1 edges  (no cycle)
- v∈V ∑​deg(v)=2|E|
- if divide degree of vertices by 2 and E is not whole, we cannot have a graph
- Cycle means DFS has back edges. 
	- n-1 edges is the number of edges that will have no cycle 
	- edges - (n-1) = number of back edges
- Worst case running times n vertices, m edges
	- Edge list: O(m)
	- Adjacency list: O(deg(v))
	- Adjacency matrix: O(n)
- Adding a vertex to a graph is O(n)
- with a pointer, it is O(1)
- breadth first search on matrix: O(n^2)
- in BFS,
	- number of edges from s -> x is at most one more than edges from s-> y 
	- there is path from x to y. 
- Cross-edges, same as back edges = edges - (n-1)
![[Pasted image 20260410152409.png]]

- complexity of BFS/DFS with adjacency list is $\Theta(m+n)$
- both bfs and dfs can find cycles
- discovery node is a node that is only visited once 

- BFS/DFS complexity 
	- adjacency list: O(V+E)
	- Adjacency matrix: O(V^2)
	- Edge list O(VE)