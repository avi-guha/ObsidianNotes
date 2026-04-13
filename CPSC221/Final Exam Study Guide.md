# CPSC 221 Final Exam Study Guide

Based on the markdown notes available in this folder: Weeks 3, 4, 5, 6, 8, 9, 10, 11, 12, and 13. `Week 7.md` was not present, so this guide reflects only the material in the files that exist.

## High-Yield Runtime Reference

| Topic                                               | Core runtime                                                             |
| --------------------------------------------------- | ------------------------------------------------------------------------ |
| Linked-list traversal                               | $O(n)$                                                                   |
| Insert/remove after pointer is known in linked list | $O(1)$                                                                   |
| Stack with linked list                              | push/pop $O(1)$                                                          |
| Stack with dynamic array                            | pop $O(1)$, push worst case $O(n)$, amortized $O(1)$ if capacity doubles |
| Queue with circular array                           | enqueue/dequeue $O(1)$                                                   |
| Binary search                                       | $\Theta(\log n)$                                                         |
| Merge sort                                          | $\Theta(n \log n)$                                                       |
| Binary tree traversal                               | $\Theta(n)$                                                              |
| BST search/insert/remove                            | $O(\text{height})$; worst case $O(n)$                                    |
| AVL search/insert/remove                            | $O(\log n)$                                                              |
| B-tree search                                       | $O(mh)$ where $m$ is order; with fixed $m$, height is $O(\log_m n)$      |
| Hash table expected performance                     | near $O(1)$ when load factor is controlled and hashing is good           |
| Hash table worst case                               | $O(n)$                                                                   |
| Heap insert/remove                                  | $O(\log n)$                                                              |
| buildHeap                                           | $O(n)$                                                                   |
| Heapsort                                            | $O(n \log n)$ time, $O(1)$ extra array space                             |
| Disjoint set with naive array representation        | Find $O(1)$, Union $\Theta(n)$                                           |
| Disjoint set with uptrees                           | Find depends on height; notes give average $O(\log n)$, worst $O(n)$     |
| BFS / DFS with adjacency list                       | $\Theta(V + E)$                                                          |
| BFS / DFS with adjacency matrix                     | $\Theta(V^2)$                                                            |
| Topological sort (queue + in-degrees)               | $\Theta(V + E)$                                                          |
| Prim with heap                                      | $O(E \log V)$                                                            |
| Dijkstra with indexed heap                          | $O((V + E)\log V)$                                                       |

## Week 3: Pointers and Linked Lists

**Key Topics**
- Pointer semantics: `&` creates an address, `*` dereferences; pointer type determines how memory is interpreted.
- Dynamic memory: `new` allocates, `delete` frees a single object, `delete[]` frees an array.
- Memory bugs:
  - Dangling pointer: pointer still refers to freed memory.
  - Memory leak: allocated memory becomes unreachable.
- Singly linked list node: stores data plus pointer to next node.
- Traversal must use a temporary pointer, not overwrite the head pointer.
- Singly linked list insertion/removal changes links locally, but finding the position is usually the expensive part.
- Variations:
  - Head pointer only.
  - Head + tail pointer.
  - Doubly linked list.
  - Circular list.
  - Sentinel nodes to eliminate edge cases.

**Runtimes**
- Traverse list of length $n$: $O(n)$.
- Insert after a known node: $O(1)$.
- Remove after predecessor is known: $O(1)$.
- Insert at back with only head pointer: $O(n)$.
- Insert at back with tail pointer: $O(1)$.
- Remove from back in singly linked list: $O(n)$ because predecessor must be found.
- Recursive reverse-print follows
$$
T(n) = T(n-1) + O(1)
$$
so the runtime is $O(n)$.

**Proof / Reasoning Ideas**
- Why traversal is $O(n)$: one pointer hop per node.
- Why reverse-print by repeated traversal is
$$
n + (n-1) + \cdots + 1 = \Theta(n^2)
$$
- Why recursive reverse-print is linear: each node contributes one recursive call and one print.
- Why sentinel nodes help: first/last insertion and removal become the same pointer update pattern as interior cases.

## Week 4: Stacks, Queues, Dynamic Arrays, Merge Sort

**Key Topics**
- ADT vs data structure:
  - ADT describes behavior and operations.
  - Data structure describes physical implementation.
- Stack is LIFO; queue is FIFO.
- Stack with singly linked list should make the list front the stack top so push/pop stay $O(1)$.
- Queue with arrays should use circular indexing rather than shifting all elements.
- Circular-array formulas:
  - Back index:
$$
(\text{front} + \text{num}) \bmod \text{capacity}
$$
  - Front advances by modular arithmetic.
- Merge sort:
  - Divide array into halves.
  - Recursively sort halves.
  - Merge sorted halves in linear time.

**Runtimes**
- Linked-list stack: push/pop/isEmpty all $O(1)$.
- Array stack:
  - pop $O(1)$
  - push worst case $O(n)$ when resize happens
  - push amortized $O(1)$ with doubling
- Circular-array queue: enqueue/dequeue $O(1)$ ignoring resize.
- Merge step on subarray of size $n$: $\Theta(n)$.
- Merge sort: $\Theta(n \log n)$.

**Proof / Reasoning Ideas**
- Merge sort recurrence:
$$
T(n) = 2T(n/2) + \Theta(n)
$$
which gives
$$
T(n) = \Theta(n \log n)
$$
- Merge sort correctness by induction:
  - Base case: array of size $0$ or $1$ is already sorted.
  - Inductive step: recursive calls sort both halves; merge combines two sorted halves into one sorted array.
  - Termination: each recursive call reduces subproblem size.
- Dynamic array amortization:
  - Increasing capacity by a constant causes total copying $\Theta(n^2)$.
  - Doubling capacity makes total copying over first $n$ pushes $\Theta(n)$, hence amortized $\Theta(1)$ per push.

## Week 5: Lower Bounds, Binary Search, Trees

**Key Topics**
- Comparison sorting can be modeled by a binary decision tree.
- Correct comparison-based sorting algorithm must distinguish all $n!$ input permutations.
- Binary search repeatedly eliminates half of a sorted array.
- Tree terminology:
  - root, leaf, parent, child, subtree, path
  - height of node/tree, depth of node
- Binary tree classes:
  - Perfect: all leaves same depth and every internal node has two children.
  - Complete: all levels full except possibly last, and last level filled left to right.
  - Full: every node has $0$ or $2$ children.
- Standard traversals:
  - inOrder
  - preOrder
  - postOrder

**Runtimes**
- Binary search: $\Theta(\log n)$.
- Any full traversal of a binary tree: $\Theta(n)$.
- Recursive tree traversal space: $O(\text{height})$.

**Proof / Reasoning Ideas**
- Lower bound on comparison sorting:
  - Decision tree needs at least $n!$ leaves.
  - Binary tree of height $h$ has at most $2^h$ leaves.
  - Therefore
$$
2^h \ge n!
$$
so
$$
h \ge \log_2(n!) = \Omega(n \log n)
$$
  - Conclusion: no comparison sort beats $\Omega(n \log n)$ in the worst case.
- Perfect-tree formulas:
  - Level counts double each level.
  - Nodes:
$$
1 + 2 + 4 + \cdots + 2^h = 2^{h+1} - 1
$$
  - Leaves:
$$
2^h
$$
- Tree facts worth memorizing:
  - Any tree with $n$ nodes has $n-1$ edges.
  - A tree of height $h$ has at least $h+1$ nodes.

## Week 6: Dictionary ADT, BSTs, Traversal, Null-Pointer Counting

**Key Topics**
- Dictionary ADT stores key-value pairs; Set ADT stores keys only.
- Recursive and iterative tree traversals:
  - stack simulates recursion
  - queue gives level-order traversal
- BST property:
  - all keys in left subtree are smaller
  - all keys in right subtree are larger, or larger/equal depending on convention
- In-order traversal of a BST yields sorted order.
- BST operations:
  - search
  - insert
  - find min/max
  - remove
- BST removal cases:
  - leaf
  - one child
  - two children

**Runtimes**
- Tree traversal: $\Theta(n)$.
- Traversal extra space: $O(\text{height})$ for recursion or explicit stack.
- BST search/insert/remove: $O(\text{height})$.
- Balanced BST: $O(\log n)$.
- Worst-case degenerate BST: $O(n)$.
- Find min/max in BST: $O(\text{height})$.

**Proof / Reasoning Ideas**
- Null-pointer theorem for binary trees:
  - A binary tree with $n$ nodes has $2n$ child-pointer slots.
  - Exactly $n-1$ of them are non-null because every node except the root has one parent edge.
  - Therefore
$$
2n - (n-1) = n+1
$$
null pointers.
- Inductive version of the same theorem:
  - Base case $n=0$: empty tree has one null.
  - Inductive step on left and right subtrees gives
$$
n_L + 1 + n_R + 1 = n + 1
$$
- Why in-order traversal of BST is sorted:
  - By induction, left subtree outputs sorted smaller keys, then root, then sorted larger keys from right subtree.

## Week 8: BST Removal Details and AVL Trees

**Key Topics**
- Removing a BST node with two children uses either:
  - in-order predecessor = rightmost node in left subtree
  - in-order successor = leftmost node in right subtree
- Predecessor/successor is chosen because it preserves BST ordering and has at most one child at the point of removal.
- Balanced trees keep height small enough to guarantee fast search/update.
- AVL tree condition: for every node, left and right subtree heights differ by at most $1$.
- Rotations preserve BST order while changing shape.
- Four imbalance types:
  - LL
  - LR
  - RR
  - RL
- AVL insertion:
  - regular BST insertion
  - backtrack
  - rotate at first critical imbalance
- AVL deletion:
  - regular BST deletion
  - backtrack and possibly rebalance at multiple nodes

**Runtimes**
- BST removal: $O(\text{height})$.
- Finding predecessor/successor in a BST: $O(\text{height})$; worst case $O(n)$, balanced case $O(\log n)$.
- AVL search/insert/remove: $O(\log n)$.
- Single rotation: $O(1)$.
- AVL deletion may rebalance at multiple ancestors, but total remains $O(\log n)$.

**Proof / Reasoning Ideas**
- AVL height proof sketch:
  - Let $N_h$ be the minimum number of nodes in an AVL tree of height $h$.
  - Minimal AVL tree of height $h$ has one subtree of height $h-1$ and one of height $h-2$.
  - So
$$
N_h = 1 + N_{h-1} + N_{h-2}
$$
  - This Fibonacci-like growth is exponential in $h$.
  - Therefore height is logarithmic in node count:
$$
h = O(\log n)
$$
- Why predecessor works in deletion:
  - It is the largest key smaller than the node being removed.
  - Replacing the deleted node with it keeps all left keys smaller and all right keys larger.
- Why rotations preserve BST property:
  - They change local parent/child relationships without changing in-order key order.

## Week 9: B-Trees and Motivation from External Memory

**Key Topics**
- AVL trees are asymptotically good in RAM, but for massive datasets disk/page accesses dominate.
- B-trees group many keys into one node so one disk page fetch yields many comparisons.
- B-tree of order $m$:
  - up to $m-1$ keys per node
  - up to $m$ children
  - leaves all at same depth
  - non-root internal nodes have between $\lceil m/2 \rceil$ and $m$ children
- Search scans keys inside a node, then descends to one child.
- Insertion:
  - search to the target leaf
  - insert if there is room
  - if full, split node and promote median upward

**Runtimes**
- Search within a node: $O(m)$ with linear scan.
- Total search: $O(m \cdot \text{height})$.
- With fixed order $m$, search is effectively logarithmic in the number of keys because
$$
\text{height} = O(\log_m n)
$$

**Proof / Reasoning Ideas**
- Height bound:
  - To maximize height for fixed $n$, minimize branching and keys.
  - Let
$$
t = \left\lceil \frac{m}{2} \right\rceil
$$
  - Root has at least $2$ children if non-leaf; lower internal levels have at least $t$ children.
  - This gives a geometric lower bound on the number of nodes/keys at height $h$.
  - Solving that bound yields
$$
h = O(\log_m n)
$$
- Main intuition:
  - Bigger node fan-out means fewer levels.
  - Fewer levels means fewer disk seeks.

## Week 10: Hash Tables, Priority Queues, Heaps, Heapsort

**Key Topics**
- Hash table basics:
  - use the entire key
  - if using modulo hashing, choose prime table size
  - collisions are inevitable
- Collision handling:
  - Open addressing
  - Separate chaining
- Open-addressing probe schemes:
  - Linear probing:
$$
(h(x) + p) \bmod \text{capacity}
$$
  - Quadratic probing:
$$
(h(x) + p^2) \bmod \text{capacity}
$$
  - Double hashing:
$$
(h(x) + p \cdot h_2(x)) \bmod \text{capacity}
$$
- Load factor:
$$
\lambda = \frac{n_{\text{items}}}{\text{table size}}
$$
- Tombstones are needed for deletion under open addressing.
- Separate chaining tolerates larger load factors better than open addressing.
- Priority queue ADT supports efficient removal of min/max priority item.
- Binary heap:
  - complete binary tree
  - partially ordered
  - naturally stored in array
- Array formulas for 0-index heap:
  - left child:
$$
2i + 1
$$
  - right child:
$$
2i + 2
$$
  - parent:
$$
\left\lfloor \frac{i-1}{2} \right\rfloor
$$
- Heap operations:
  - insert via heapify-up
  - remove root via heapify-down
  - buildHeap from unordered array
  - heapsort

**Runtimes**
- Hashing expected performance is close to $O(1)$ when keys distribute well and load factor stays controlled.
- Hash table worst case is $O(n)$.
- Open addressing becomes poor as $\lambda$ grows; notes emphasize keeping
$$
\lambda < \frac{1}{2}
$$
- Priority queue implementations:

| Structure | Insert | removeMin / removeMax |
| --- | --- | --- |
| Unordered array | $O(1)$ | $O(n)$ |
| Unordered list | $O(1)$ | $O(n)$ |
| Ordered array | $O(n)$ | $O(n)$ |
| Ordered list | $O(n)$ | $O(1)$ |
| BST | $O(n)$ | $O(n)$ in worst case |
| AVL | $O(\log n)$ | $O(\log n)$ |
| Binary heap | $O(\log n)$ | $O(\log n)$ |

- Heap insert: $O(\log n)$.
- Heap remove root: $O(\log n)$.
- Repeated insertion build: $O(n \log n)$.
- Bottom-up buildHeap: $O(n)$.
- Heapsort: $O(n \log n)$ time, $O(1)$ extra space beyond the array.

**Proof / Reasoning Ideas**
- Why linear probing clusters:
  - consecutive occupied cells create longer and longer runs, increasing future collisions.
- Why search must use the same probe scheme as insertion:
  - otherwise it may stop before reaching the actual stored position.
- Why buildHeap is $O(n)$, not $O(n \log n)$:
  - In equation form, the total cost is linear rather than
$$
O(n \log n)
$$
  - Most nodes are near the bottom and move only a few levels.
  - Sum the maximum downward edge traversals across all nodes.
  - Total is proportional to number of edges in the tree, hence $O(n)$.
- Why heapsort is $O(n \log n)$:
  - $O(n)$ to build heap plus $n$ removals, each $O(\log n)$.

## Week 11: Disjoint Sets and Graph Foundations

**Key Topics**
- Disjoint sets model partitioning items into non-overlapping groups.
- Core operations:
  - MakeSet
  - Find
  - Union
- Fundamental property: $\mathrm{Find}(x) = \mathrm{Find}(y)$ iff $x$ and $y$ are in the same set.
- Naive array representation makes Find trivial but Union expensive.
- Uptree representation stores parent pointers; root is representative.
- Smart union and path compression keep trees shallower.
- Graph basics:
  - graph $G = (V, E)$
  - directed vs undirected
  - weighted vs unweighted
  - path, cycle, degree, adjacency, connected component, spanning tree
- Graph density:
  - sparse: $O(V)$ edges
  - dense: $\Theta(V^2)$ edges

**Runtimes**
- Naive disjoint-set array:
  - Find $O(1)$
  - Union $\Theta(n)$
- Uptrees:
  - Find depends on tree height
  - notes give average $O(\log n)$, best $O(1)$, worst $O(n)$
- Graph analysis usually depends on both $|V|$ and $|E|$.

**Proof / Reasoning Ideas**
- Handshaking theorem for undirected graphs:
  - Every edge contributes $1$ to the degree count of each endpoint.
  - Therefore
$$
\sum_{v \in V} \deg(v) = 2|E|
$$
- Directed version:
$$
\sum_{v \in V} \deg^-(v) = \sum_{v \in V} \deg^+(v) = |E|
$$
- Why a tree has $n-1$ edges:
  - Standard structural property of connected acyclic graphs and a recurring exam fact.

## Week 12: BFS, DFS, Topological Sort, Spanning Trees

**Key Topics**
- Traversal differs from tree traversal because graphs can be disconnected and cyclic.
- BFS:
  - uses queue
  - explores vertices by distance layers from a start vertex
  - discovery edges form a BFS tree/forest
  - gives shortest path lengths in unweighted graphs
- DFS:
  - uses recursion or explicit stack
  - goes as deep as possible before backtracking
  - discovery edges form DFS tree/forest
  - back edges are the key cycle signal in the notes
- Traversal wrappers `Traverse_BFS` and `Traverse_DFS` handle disconnected graphs by restarting from unvisited vertices.
- Topological sort:
  - only for directed acyclic graphs
  - repeatedly output vertices of in-degree zero
- Maze construction:
  - choose edges that connect new regions without creating cycles
  - conceptually builds a spanning tree
- Spanning tree:
  - connected
  - acyclic
  - spans all vertices

**Runtimes**
- BFS with adjacency list: $\Theta(V + E)$.
- DFS with adjacency list: $\Theta(V + E)$.
- BFS/DFS with adjacency matrix: $\Theta(V^2)$.
- Edge-list representation can make traversal much worse; notes list $O(VE)$.
- Per-vertex neighbor access by representation:
  - edge list: $O(E)$
  - adjacency list: $O(\deg(v))$
  - adjacency matrix: $O(V)$
- BFS/DFS spanning tree comes "for free" with traversal, so same asymptotic cost as traversal.
- Topological sort with in-degree table and queue: $\Theta(V + E)$.

**Proof / Reasoning Ideas**
- Why BFS gives unweighted shortest paths:
  - vertices are visited in nondecreasing number of edges from the source.
  - first discovery of a vertex is through the shortest-hop path.
- Why one BFS/DFS call visits exactly one connected component:
  - traversal expands along all reachable edges from the start and no further.
- Tree characterization worth memorizing:
  - connected + $n-1$ edges implies tree
  - tree implies connected, acyclic, and exactly $n-1$ edges
- Cycle detection idea from the notes:
  - DFS back edges witness cycles.

## Week 13: MSTs and Single-Source Shortest Paths

**Key Topics**
- Spanning tree: connected subgraph with exactly
$$
|V| - 1
$$
edges.
- Minimum spanning tree (MST): spanning tree with minimum total weight.
- Kruskal's algorithm:
  - repeatedly choose minimum-weight edge that connects two different components
  - uses priority queue for edges and disjoint sets for cycle detection
- Prim's algorithm:
  - grow one tree from a start vertex
  - repeatedly add cheapest edge leaving the current tree
  - naturally uses a priority queue
- Shortest-path problem:
  - source $s$
  - compute minimum path cost from $s$ to every vertex
- Dijkstra's algorithm:
  - greedy
  - only valid for graphs with no negative edge weights
  - repeatedly finalize the unreached vertex with smallest tentative distance
  - relax outgoing edges

**Runtimes**
- Kruskal:
  - sorting or priority-queue processing of edges dominates
  - standard bound:
$$
O(E \log E)
$$
and with
$$
E \le V^2
$$
this is also
$$
O(E \log V)
$$
- Prim with heap:
  - notes derive
$$
O(E \log V)
$$
- Dijkstra:
  - initial heap build $O(V)$
  - $\mathrm{RemoveMin}$ done $V$ times: $O(V \log V)$
  - relaxing edges:
    - unindexed heap can cost as much as $O(EV)$ for updates
    - indexed heap gives
$$
O((V + E)\log V)
$$

**Proof / Reasoning Ideas**
- Kruskal correctness intuition:
  - safe edge is the cheapest one connecting two different current components.
  - adding it cannot block an optimal MST because any MST must connect those components somehow.
- Prim correctness intuition:
  - partition the graph into vertices already in the tree and those outside.
  - the minimum edge crossing that cut is safe to add.
- Dijkstra invariant:
  - once a vertex enters the cloud/reached set, its recorded distance is the true shortest-path distance.
  - proof is by induction on the number of finalized vertices.
  - negative edges break the argument because a supposedly finalized distance could later be improved.

## Proof Checklist

These are the proof patterns most worth being able to reproduce quickly on an exam.

- Merge sort correctness by induction.
- Merge sort runtime from
$$
T(n) = 2T(n/2) + \Theta(n)
$$
- Dynamic-array amortized analysis: constant increment vs doubling.
- Comparison-sorting lower bound via decision tree and $n!$ leaves.
- Perfect-tree node formula
$$
2^{h+1} - 1
$$
- Binary-tree null-pointer theorem: exactly
$$
n+1
$$
nulls.
- In-order traversal of BST gives sorted order.
- AVL height proof using
$$
N_h = 1 + N_{h-1} + N_{h-2}
$$
- B-tree height bound from minimum branching factor.
- buildHeap is $O(n)$ by summing possible downward edge traversals.
- Handshaking theorem.
- BFS shortest-path argument in unweighted graphs.
- Tree characterization: connected + acyclic, or connected + $n-1$ edges.
- Dijkstra correctness invariant and why negative weights fail.

## Final Cram Priorities

- Know which structure is best for which ADT: stack, queue, dictionary, priority queue, disjoint set.
- Be able to move between height-based and node-based reasoning for trees.
- Memorize the few formulas that keep recurring:
  - $2^{h+1} - 1$
  - $\lambda = \frac{n}{m}$
  - heap index formulas
  - $\Theta(V + E)$ for adjacency-list graph traversals
  - $O(\log n)$ for AVL operations
  - $O(n)$ for buildHeap
- For greedy graph algorithms, know the exact local choice:
  - Kruskal: cheapest edge that does not create a cycle
  - Prim: cheapest edge leaving the current tree
  - Dijkstra: unreached vertex with minimum tentative distance
