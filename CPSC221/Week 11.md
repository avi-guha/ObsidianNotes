# Disjoint Sets 

## A new ADT about sets 

- we can consider a group of people who trust each other. 
	- A trusting pair shares secrets 
	- untrusting pairs do not
- How can we determine which group or individuals secrets spread to 

## Set ADT operations 
- Operations $\textemdash$ we want: 
	- initialize a set of people: makeset 
	- Each time we learn a pair trusts each other, merge into same set: Union 
	- Determine if people share a trust group by finding which set each is in. 

# Disjoint Set ADT 
- No elements is a member of two different sets 
- Disjoint set operations
	- Create
	- Destroy
	- Union 
	- Find 
- Disjoint set property: Find(x) produces the same result as Find(y) if and only if x and y are in the same set. 
	- Union-ing of x and y causes their sets to permanents merge.

## Data structure of Disjoint Sets 
- Maintains a collection of sets $S = {s_{1},s_{2}, \dots s_{n}}$ that are all disjoint 
- Each set has a representative member (leader)
- Required operations: 
	- MakeSet
	- Union 
	- Find
- In an array based structure: 
	- Find is O(1)
	- Union $\Theta(n)$
		- Array iteration to locate all group members who need a new leader 

## A better structure of disjoint sets 
- uptrees 
- A tree where a node points to its parent 
	- still array based, but representative is the root of the tree 
		- if array value is -1, index is a root node 
		- otherwise the array value is the index's parent 
	- x and y are in the same tree $\iff$ and and y are in the same set 

## Tree-based disjoint sets 
- Running time of Find 
- Depends on the height of the trees in a disjoint set 
- Complexities: 
	- $O(\log n)$ -average
	- $O(n)$ - worst 
	- $O(1)$ - best

- Union: give arbitrary indices to x and y, join their trees.
	- Naively, set root of x to y or vice versa. 
	- Better: set root of x to root of y. 
- This can still give us a bad tree 

## Smart Union 

## Path compression
- During find operations, we follow a path up the tree through a sequence of nodes. 
	- look up number of entries in an array, each lookup is O(1)
- Why don't we add additional O(1) for each entry we process
	- set the parent of each node along the path, to the root found at one end of the path. 

# Introduction to Graphs 

## What is a graph 
- A graph, G, is a collection of vertices connected by edges. 
- Formally, a graph is a pair of sets: G = (V,E)
	- V is a set of vertices (representing anything): $V ={v_{1}, v_{2}, \dots v_{n}}$
		- cardinality of this set is n
	- E is a set of edges: $E = {e_{1},e_{2},\dots e_{m}}$
		- cardinality of this set is m 
		- each $e_{i}$ is a pair of vertices $e_{i} = V \times V$
	- if each edge is an ordered pair $(ie. (A,B) \neq (B,A))$ then the graph is directed. 
		- otherwise it is undirected. 
## Graph Applications 
- storing things that are networks by nature. 
	- Road networks
	- Airline flights/ connections 
	- relationships between people / things 
- Compilers 
	- Call graph - which functions call what 
	- dependancy graph - which variables depend on each other 
- Others 
	- Circuits, class hierarchies, computer networks etc. 

# Graph Applications: 
- GPS navigation
	- Vertices are cities
	- Edges are highways that connect cities with driving distance. 
- Exam Scheduling
	- vertices are courses
	- edges at least one students is registered in both courses at the end points
- State space graph 
	- vertices are an arrangement of the game board 
	- A valid move that transforms one arrangement to another 

## Graph Vocabulary 
- Vertices adjacent to v: $N(v) = {u|(u,v) \in E}$
- Edges incident to v: $I(v) = {(u,v) | u \in N(v)}$
- Degree of v: $deg(v) = |I(v)|$
- Path: sequence of vertices connected by edges 
- Simple Path: a path with no repeated vertices 
- Cycle: Path with same start and end vertex 
- Simple graph: no self-loops or multi-edges. 
- Subgraph of $G = (V,E): (V' \subseteq V, E' \subseteq E)$ if $(u,v) \in E'$ then $u,v \in V'$
- Complete graph: maximum number of edges (simple graph)
	- every vertex connected by edge to every other vertex 
- Connected graph: Path exists between every pair of vertices 
	- not necessarily an edge between every pair of vertices 
- Unconnected graph: there is some pair of vertices for which there is no path between them
- Connected components: maximal connected  subgraph, all vertices which have = path between them (in same part of the graph)
- Acyclic graph: no cycles anywhere in the graph. 
- Spanning tree of $G = (V,E)$: Acyclic, connected graph with vertex set V. Acyclic, connected subgraph of G, where 

## Weighted Graphs 
- in a weighted graph each edge is assigned a weight 
	- edges are labeled with their weights 
- Each edge's weight represents the cost to travel along that edge 
	- the cost could be distance, time, money or some other measure 
	- the cost depends on the underlying problem 

## Connectivity 
- Undirected graphs are connected if there is a path between any two vertices 
- Directed graphs are strongly connected if there is a directed path from any vertex to any other 
- Diagraphs (directed graphs) are weakly if there is a path between any two vertices, ignoring direction 
- A complete graph has an edge between every pair of vertices 
	- max number of edges for a simple graph.

## Isomorphism and Subgraphs 
- We often care only bout the structure of a graph, not the names of its vertices 
	- Are two graphs "isomorphic?", do the two graphs have identical structure / can we line up the vertices so that the edges match? 
	- Is on graph a subgraph of another? is one graph isomorphic to a part of the other graph (a subset of vertices and a subset of edges connecting those vertices)
	- $G' = (V',E')$, $V' \subseteq V, E' \subseteq E$ if $(u,v) \in E'$ then $u,v \in V'$

	- Example applications: 
		- identifying chemical compounds in a chemical database. 
		- determining whether two (physically) different circuit layouts are electrically equivalent 

## Degree 
- The degree of a vertex $v \in V$ is denoted $deg(v)$ and represents the number of edges incident on $v$.
- Handshaking theorem: 
	- If $G = (V,E)$ is an undirected graph, then 
	- $\sum^{}_{v \in V} deg(v) = 2|E|$


## Degree for directed graphs
- The in-degree of a vertex $v \in V$ is denoted $deg^{-}(v)$ and is the number of edges entering v. 
- The out-degree of a vertex $v \in V$ is denoted $deg^{+}(v)$ and is the number of edges leaving $v$.

- We let $deg(v) = deg^{+}(v) + deg^{-}(v)$ Then, 
- $\sum^{}_{v \in V} deg^{-}(v) = \sum^{}_{v \in V} deg^{+}(v) = \frac{1}{2}\sum^{}_{v \in V} deg(v) = |E|$

## Graph Density 
- A sparse graph has $O(|V|)$ edges.
- A dense graph has $\Theta(|V^2|)$ edges. 
	- Anything in between is either on the sparse side or on the dense side, depending critically on context. 
- Analysis of graph operation typically must be expressed in terms of both |V| and |E|.
