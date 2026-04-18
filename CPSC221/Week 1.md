
## Data structures and algorithms 
- We can get a variety of solutions for a problem 
	- Each solution can be described by a detail of steps - algorithm
	- Each solution depended on the existence of certain hardware or organization of the collection - data structure 
- Each solution produces the same solution 
	- But they may all take different amounts of time or same solution, locates the specified person 
	- need a way to compare different solutions 

## Methods and comparing algorithms 
- If a algorithm is not correct, is its value zero?
- A "good" algorithm should: 
	- produce correct solution 
	- finish in reasonable time 
		- time complexity 
	- use a 'reasonable' amount of system memory 
		- space complexity 
- Running time, expressed as a function T(n): $Z^{0} \to R^0$
- n is the collection size 
	- finding your friend in a tutorial section vs. at the music festival

## Running times 
![[Pasted image 20260414144240.png]]

# Reasoning about data structures and algorithms
## Algorithm Analysis 
- Analysis of an algorithm gives insight into 
	- correctness 
	- how long it takes to run

## Analyzing Algorithms 
- What do they do? 
- How long do they take?

## Asymptotic Notation 
- $T(n) \in O(f(n))$ if there are constants c and $n_{o}$ such that $T(n) \leq c$ $f(n) \forall n> n_{o}$
- ![[Pasted image 20260414144902.png]]


## O-notation visually 
- $T(n) \in O(F(n)) if \exists c \cap n_{0} s.t T(n) \leq cf(n) \forall n\geq n_{o}$

## Why?
- O notation allows us to describe an upper bound on the growth behaviour of an algorithm's running time function T(n)
- Consider the running time of a function 
- we can demonstrate that our running time is O(bigger function)  but not useful 
- But demonstrating this with a tighter bound give us more info. 
- The smaller the reference function that satisfies the O relation is called a tight upper bound. 

## Other notation 
|**Notation**|**Formal Definition**|**Notes**|
|---|---|---|
|$T(n) \in O(f(n))$|if there are constants $c$ and $n_0$ such that $T(n) \leq c \cdot f(n)$ for all $n \geq n_0$|$f(n)$ is an upper bound on $T(n)$|
|$T(n) \in \Omega(f(n))$|if there are constants $c$ and $n_0$ such that $T(n) \geq c \cdot f(n)$ for all $n \geq n_0$|$f(n)$ is a lower bound on $T(n)$|
|$T(n) \in \Theta(f(n))$|if $T(n) \in O(f(n))$ and $T(n) \in \Omega(f(n))$|$f(n)$ is a tight bound on $T(n)$|
|$T(n) \in o(f(n))$|if for **any** positive constant $c$, there exists $n_0$ such that $T(n) < c \cdot f(n)$ for all $n \geq n_0$||
|$T(n) \in \omega(f(n))$|if for **any** positive constant $c$, there exists $n_0$ such that $T(n) > c \cdot f(n)$ for all $n \geq n_0$|
# Asymptotic Analysis 

| **Name**    | **Big-O Notation** | **Notes**                                                            |
| ----------- | ------------------ | -------------------------------------------------------------------- |
| Constant    | $O(1)$             |                                                                      |
| Logarithmic | $O(\log n)$        | Not necessary to specify base. $(\log_k n, \log(n^2) \in O(\log n))$ |
| Poly-log    | $O((\log n)^k)$    |                                                                      |
| Sublinear   | $O(n^c)$           | ($c$ is a constant, $0 < c < 1$)                                     |
| Linear      | $O(n)$             |                                                                      |
| Log-linear  | $O(n \log n)$      |                                                                      |
| Superlinear | $O(n^{1+c})$       | ($c$ is a constant, $0 < c < 1$)                                     |
| Quadratic   | $O(n^2)$           |                                                                      |
| Cubic       | $O(n^3)$           |                                                                      |
| Polynomial  | $O(n^k)$           | ($k$ is a constant) "tractable"                                      |
| Exponential | $O(c^n)$           | ($c$ is a constant $> 0$) "intractable"                              |
| Factorial   | $O(n!)$            |                                                                      |
