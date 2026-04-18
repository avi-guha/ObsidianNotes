# Correctness 

## Proving Correctness 
- need to formally prove correctness 
- using loop invariants 

## Induction 
- Correctness of algorithm can be argued formally with a loop invariant 
- Loop invariant 
	- properties of the algorithm or structure that are always true at particular points in the program 
	- within a loop body, these properties may become violated briefly, but the rest of the loop instructions should fix the properties for the next iteration 
	- A loop invariant proof must use features of code being analyzed to demonstrate that any violations of loop invariant are restored 
## Induction and loop invariants 
- ![[Pasted image 20260417215316.png]]
- We start with concrete example.
- suppose we pause at the start of some iteration, i
	- what do we know about state of the local variables 
- proving loop invariant: 
	- Induction variable: 
		- number of times through the loop
	- Base case: 
		- Directly prove the invariant holds before loop starts
	- Induction hypothesis 
		- Assume invariant holds before beginning some unspecified loop iteration 
	- Termination: 
		- ensure the invariant implies correctness when loop ends


Here are the notes on Selection Sort and Insertion Sort, formatted in Markdown so you can easily copy and paste them directly into Obsidian.

# Simple Iterative Sorting Algorithms

**Tags:** `#algorithms` `#sorting` `#selection-sort` `#insertion-sort` `#computer-science`

## 1. Selection Sort

### Concept

Selection sort is a simple algorithm that divides an array into a **sorted part** and an **unsorted part**.

- The algorithm repeatedly scans the unsorted section to find the smallest item.
- Once found, it swaps this smallest item with the first element of the unsorted part, effectively expanding the sorted section by one.

### Loop Invariant

- Before iteration $i$ of the loop, the subarray `arr[0..i-1]` contains the $i$ smallest elements of the array in strictly ascending order.

### Performance Analysis

- **Time Complexity:**
    - Makes exactly $n(n-1)/2$ comparisons, regardless of the original order of the input.
    - Runtime is **$O(n^2)$** in the Best, Average, and Worst cases.
- **Swaps / Writes:**
    - Performs exactly $n-1$ swaps.
    - **Best Use Case:** Useful in extremely limited systems where reading data is cheap, but writing (swapping) data is expensive.
- **Memory:** **$O(1)$** constant space.

### Example Walkthrough

Given the starting array: `23 41 33 81 7 19 11 45`.

- **Step 1:** Find smallest unsorted item (**7**) and swap with first element (7 comparisons).
    - _Result:_ `7 41 33 81 23 19 11 45`
- **Step 2:** Find smallest unsorted item (**11**) and swap (6 comparisons).
    - _Result:_ `7 11 33 81 23 19 41 45`
- **Step 3:** Find smallest unsorted item (**19**) and swap (5 comparisons).
    - _Result:_ `7 11 19 81 23 33 41 45`
- **Step 4:** Find smallest unsorted item (**23**) and swap (4 comparisons).
    - _Result:_ `7 11 19 23 81 33 41 45`
- **Step 5:** Find smallest unsorted item (**33**) and swap (3 comparisons).
    - _Result:_ `7 11 19 23 33 81 41 45`
- **Step 6:** Find smallest unsorted item (**41**) and swap (2 comparisons).
    - _Result:_ `7 11 19 23 33 41 81 45`
- **Step 7:** Find smallest unsorted item (**45**) and swap (1 comparison).
    - _Final Result:_ `7 11 19 23 33 41 45 81` (Sorted)

---

## 2. Insertion Sort

### Concept

Insertion sort also divides the array into **sorted** and **unsorted** parts.

- It expands the sorted part of the array one element at a time.
- It takes the first element of the unsorted part and searches backward through the sorted elements to find its correct position.
- Elements are moved (slid) up one position to make space, and the new element is inserted into its proper place.

### Loop Invariant

- Before processing index $i$, the subarray `arr[0..i-1]` contains the first $i$ elements of the original array, arranged in sorted order.

### Performance Analysis

- **Best Case:** **$O(n)$**. Occurs when the array is already completely sorted. It requires 0 moves and only $n$ comparisons.
- **Worst Case:** **$O(n^2)$**. Occurs when the array is in reverse order. Every item must be moved all the way to the front, resulting in $n(n-1)/2$ comparisons and moves.
- **Average Case:** **$O(n^2)$**. Performs around $n(n-1)/4$ comparisons, which places its performance closer to the worst case for random data.
- **Memory:** **$O(1)$** constant space.
- **Best Use Case:** Highly efficient when the data is already _nearly sorted_ (few elements out of place), or when the problem size is small due to its low overhead.

### Example Walkthrough

Given the starting array: `23 41 33 81 7 19 11 45`.

- **Step 1:** The first element (`23`) is already considered sorted.
- **Step 2:** Locate position for **41** (1 comparison). Stays in place.
    - _Result:_ `23 41 33 81 7 19 11 45`
- **Step 3:** Locate position for **33** (2 comparisons). Slides backward.
    - _Result:_ `23 33 41 81 7 19 11 45`
- **Step 4:** Locate position for **81** (1 comparison). Stays in place.
    - _Result:_ `23 33 41 81 7 19 11 45`
- **Step 5:** Locate position for **7** (4 comparisons). Slides to the very front.
    - _Result:_ `7 23 33 41 81 19 11 45`
- **Step 6:** Locate position for **19** (5 comparisons). Slides backward.
    - _Result:_ `7 19 23 33 41 81 11 45`
- **Step 7:** Locate position for **11** (6 comparisons). Slides backward.
    - _Result:_ `7 11 19 23 33 41 81 45`
- **Step 8:** Locate position for **45** (2 comparisons). Slides backward.
    - _Final Result:_ `7 11 19 23 33 41 45 81` (Sorted)