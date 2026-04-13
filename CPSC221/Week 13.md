# Minimum spanning trees 

## Spanning Trees 
- Given $G = (V,E)$, a spanning tree of G is a connected subgraph of G with exactly $|V| -1$ edges 
	- a minimal subset of edges that connects all the vertices of G 

## Minimum Spanning Trees 
- Finding the minimum configuration of something that connects all nodes 
- To solve this problem, we need a minimal spanning tree 

- Given a connected graph $G = (V,E)$ with unconstrained edge weights (can be positive or negative) 
- Output a graph $G' = (V, E')$ with the following characteristics: 
	- G' is a spanning subgraph of G 
	- G' is connected and acrylic (a tree)
	- The sum of edge wrights of $E'$ is minimal amongst all such spanning trees 

## Kruska's Algorithm 
- Builds a spanning tree from several connected components 
- Repeatedly chooses the minimum-weight joining two connected components, which does not form a cycle, until edge set has $|V| -1$ edges 

``` c; 
ruskalsAlgorithm()
{
set 𝐸′ = ø
while ( 𝐸′ ≠ 𝑉 − 1)
{
Find minimum weight edge 𝑒 ∉ 𝐸′ such that 𝐸′ ڂ 𝑒 does not contain cycles
Add 𝑒 to 𝐸′
}
}
```

- We need ADTs that support our required operations efficiently 
	- we can find the minimum edge weight with a priority queue 
	- we can check for cycles and perform unions with disjoint sets 

**similar to maze under construction**
- Random edge for maze construction is like minimum weight edge for MST 

## Prim's Algorithm 
- Based on partition property in graphs 
- builds a spanning tree from initially one vertex 
- Repeatedly choose minimum weight edge from a vertex in the tree, to a vertex outside the tree then adds that vertex to the tree. 
```c;
PrimsAlgorithm(v)
{
mark v as visited, add v to spanning tree
while (graph has unvisited vertices)
{
Find least cost edge (w, u) from a visited vertex w to unvisited vertex u
Mark u as visited
Add vertex u and edge (w, u) to the minimum spanning tree
}
}

```

- Unlike kruskal's algorithm we will intersperse insertion and removal operations to the priority queue 
- Maximum number of insertions to the priority queue 
	- Assuming heap implementation 
	- |E| in the worst case, then total cost of all insertions is $O(|E|\log|E|)$
	- For dense graphs, $|E| \in O(|V|^2)$, then $\log|E| \in O(2 \cdot \log|V|) \in O(\log|V|)$
	- Thus the complexity of Prim's algorithm is $O(|E|\log|V|)$

#  Single - Source Shortest Path 

## Traversal variants 
- Traversal using stack / recursion, add unvisited neighbour 
	- DFS 
- Traversal using queue, add unvisited neighbour 
	- BFS 
- Traversal using queue, add zero-degree neighbour 
	- Topological sort 
- Traversal using priority queue (edge weights), add all neighbours 
	- Prim's algorithm 
- Traversal using priority queue (path weights), update all neighbours 
	- Djikstra's algorithm 


## Single Source Shortest path 
- Given a weights graph $G = (V,E)$ and a starting vertex $s \in V$, find the shortest path from s to every vertex in V 
- Variations 
	- Unweighted vs. weighted 
	- Cyclic vs acyclic 
	- Positive weights only vs negative weights allowed 
	- Multiple weight types to minimize 

**Example**
- least cost path from one vertex to another 
	- For weighted graphs, the lowest cost path that is the smallest sum of its edge weights 
	- ![[Pasted image 20260413004439.png]]
	- Shortest path between B and G is not direct, it is B-D-E-F-G

## Dijkstra's algorithm
- Classic algorithm for solving shortest path in weighted graphs without negative weights 
- It is greedy 
- Best choice is made locally at each step without considering future consequences 
- Ideology 
	- Shortest path from a source vertex to itself is 0 
	- Cost of going toa djacent nodes is at most edge weights 
	- cheapest of these must be shortest path to that node 
	- Update paths for a new node and continue picking shortest path. 

``` c;
Dijkstra(G,s)
	d(s) = 0
	for all u ∈ V – {s}, d(u) = ∞
	R = ∅
		while R ≠ V
		pick u ∉ R with minimal d(u)
		R = R ∪ {u}
		for all vertices v adjacent to u
			if d(v) > d(u) + l(u,v)
				d(v) = d(u) + l(u,v)
```

**Initialization**
- set distance to source = 0 
- set distance to other vertices to infinity 
- set of reached vertices = empty 

**While there are unreched vertices**
- choose unvisited vertex u with minimal cost to reach 
- For all neighbours v of u. 
	- if path to v through u has lower cost than current known shortest path update shortest cost path v. 

- Once the results array is complete paths from the start vertex can be retrieved 
- Done by looking up the end vertex (the vertex to which one is trying to find a path) and backtracking through parent vertices to the start 
- For example, to find a path to vertex E backtrack through: 
	- E -> G -> F -> B -> C
	- Note: there should be some efficient way to search the results array for a vertex 
	- and the path in reverse order 

## Data structures for Djikstra's algorithm 
- Selecting unvisited node with the minimum cost 
	- Priority queue / min heap. 
	- building initial heap is $O(|V|)$
	- RemoveMin executed |V| times, total of $O(|V|\log|V|)$
- Each of |E| edges has to be processed once 
	- looking up and changing the current cost of vertex in a heap takes O(|V|) for an unindexed heap (O(1) if the heap is indexed)
		- The heap property needs to be preserved after a change for an additional cost $O(\log|V|)$
			- or $O(|V\log|V| +|E||V|)$
		- If the heap is indexed the cost is $O((|V| +|E|)\log|V|)$

## Correctness 
- Invariant: vertices in the cloud have a known shortest path, and u is the next vertex to add according to the priority queue 
- Proof by induction, note that the negtaive edge weights can wreck this proof. 
![[Pasted image 20260413005840.png]]

