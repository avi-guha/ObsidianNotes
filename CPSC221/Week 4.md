
# Stack ADT
- ADT is a conceptual view of a data collection 
	- often have operations, we study the effect of these operations on the conceptual view 
- Data structures are the physical organization of a data collection in computer memory. 

## Stacks in practical Usage
- Action happens at the top of the stack 

## Stack ADT 
- ADT describes data collection and the operations that can be performed on the collection 
	- The effects of operations are well understood 
	- How the operations are implemented are not described. 
``` c;
template <class LIT>
class Stack {
public:
Stack(); // + copy, destructor etc.
	bool IsEmpty() const;
	void Push(const LIT& e); // insert at top of stack
	LIT Pop(); // remove and return from top of stack
private:
// ???
};
```
- Private members will reveal some details about implementation, but knowledge of only public interface allows full usage and interaction. 
- ![[Pasted image 20260207093013.png]]

## Stack Implementation 
- Make the front of the list serve as the top of the stack
	- All operations are done in O(1) time. 
- We use a null terminated front pointer. 
- If the null-terminated pointer is at the back of the list this is O(n) push and pop time.
- ![[Pasted image 20260207093136.png]]

- We can implement a stack using a singly linked list. 
```c;
template <class LIT>
class Stack {
	public:
		Stack();
		bool IsEmpty() const;
		void Push(const LIT& e);
		LIT Pop();
	private:
		struct Node {
		LIT data;
		Node* next; };
		Node* top;
		int size;
};
```
```c;
template <class LIT>
void Stack<LIT>::Push(LIT d) {
	Node* newnode = new Node(d);
	newnode->next = top;
	top = newnode;
}
```
``` c;
template <class LIT>
bool Stack<LIT>::IsEmpty() const {
	return top == nullptr;
}
```
```c;
template <class LIT>
LIT Stack<LIT>::Pop() {
	assert(!IsEmpty());
	LIT ret = top->data;
	Node* temp = top;
	top = top->next;
	delete temp;
	return ret;
}
```
- We can use an array
	- make the most recently occupied index the top of the stack. 

## Array Resizing 
- vectors are able to resize, but we need more specific control over how it resizes. 
- For example, if we copy over n push operations the time complexity is $O(n^2)$, we can malloc more space for the array instead to speed this up. 
- ![[Pasted image 20260207095833.png]]

### Summary 
- Linked list implementation 
	- O(1) for all implementations 
- Array based implementation 
	- O(1) pop
	- O(n) push worst case 
	- Cost over O(n) pushes is O(n) for an average of O(1) per push 
- We can use an array if we are concerned about cache performance. 

# Queues 
- Queue items are inserted at the back and removed form the front 
- Queues are FIFO data structures 
- Applications include:
	- Server requests 
		- instant messaging servers queue up incoming messages 
		- database request
	- Print queues 
	- operating systems use this to schedule CPU jobs 
	- Various algorithm implementations

## Queue Operations 
- A queue should have at least the first two 
	- enqueue - insert item at back of the queue 
	- Dequeue - remove item at the front 
	- peek - return item at front without removing 
	- is_empty -check if the queue does not contain any items 
- Like stacks, assume that these are implemented efficiently. 

## Queue Implementation 
- We can consider an array as the underlying structure for a queue, we could do the following:
	- Make the back of the queue the current size of the array, like the stack implementation 
	- Initially make the front of the queue index 0. 
	- Inserting is easy 
- If we want to remove the items with array implementation 
	- Moving all elements down is slow.
	- Incrementing front index wastes space. 

## Circular Arrays 
- Trick: use circular array to insert and remove items from a queue in constant time 
- The idea of a circular array is that the end of the array wraps around to the start. 
- ![[Pasted image 20260207101402.png]]

## The Modulo Operator 
- The mod operator (%) calculates remainders 
- The mod operator can be used to calculate the front and back positions in a circular array. 
	- We avoid comparisons to the array size. 
	- Back of the queue is defined as:
		- $(front + num) \; \% \; capacity$
	- The front of the queue is defined as: 
		- $(front + 1) \; \% \; capacity$

## Array Queue Resizing 
- Suppose we have an array based queue and we have performed some enqueue and dequeue operations 
	- Then we perform more enqueues to fill the array. 
	- We have two options to resize the array 
		- Reset front index to 0. 
		- Keep front at the same index. 
	- We can also just allocate more space in memory for it. 

## Merge Sort 
- Repeatedly divide arrays in half until each subarray contains a single element 
	- merging two single element arrays is a single comparison 
- The merge step copies the subarray halves into a temporary array 
	- The merged elements are copied from the temporary array back to original array

```c;
void MergeSort(vector<T>& arr) {
MSort(arr, 0, arr.size() – 1);
}
void MSort(vector<T>& arr, int low, int high) {
	if (low < high) { // array has more than 1 element
		int mid = (low + high) / 2;
	// sort the left half
		MSort(arr, low, mid);
		// sort the right half
		MSort(arr, mid+1, high);
		// Merge the sorted halves
		Merge(arr, low, mid, high);
	}
}
```

### Merge Sort Analysis 
- We want to understand how many compairsons are made in the merge step 
	- Worst case: n-1 comparisons 
		- need to check every subarray index 
	- Best case: n/2 comparisons 
		- Reach the end of one subarray, copy the rest of the second subarray 
- Still copying n subarray items in any case.
- We also want to know how many times each subarray can be divided 
	- $\log_{2}n$ divisions to reach 1 element subarrays 
- Overall, this is $\Theta(n\log n)$
- We can prove that $T(n) \in O(n\log n)$

## Merge Sort Correctness 

``` c;
void MergeSort(vector<T>& arr) {
MSort(arr, 0, arr.size() – 1);
}
void MSort(vector<T>& arr, int low, int high) {
	if (low < high) { // array has more than 1 element
	int mid = (low + high) / 2;
	MSort(arr, low, mid);
	MSort(arr, mid+1, high);
	Merge(arr, low, mid, high);
	}
}
```
- Proof by induction, not a loop invariant 
	- Claim is that MSort will sort an array
- Base case is n=0 or n=1, array is empty or size 1, already sorted. 
	- low == high+1 or low == high, condition fails and function returns without modifying array.
- Inductive step: consider arbitrary 1<k<= arr.size(), assume that MSort sorts an array for 0 < high-low+1 \< n 
	- Show that it sorts for all high - low + 1 = n
- Termination 
	- In the recursive case, low < high and low < $\frac{low+high}{2}$ < high and each 'half' has at least one element and at most n-1. Thus in a finite number of recursive calls, we reach the base case. 

## Summary of Sorting Algorithms 
![[Pasted image 20260207192239.png]]

- Big question: are there other comparison based sorting algorithms that can do better than merge sort, in the worst case? 
	- Comparison-based algorithms can compare two elements in constant time 
	- Cannot manipulate array elements in any other way 
		- cannot assume that array elements are numbers and perform arithmetic operations such as division on them

Quiz preparing: 

**Recursive Defintions**
- divide into k tasks and merge in constant time 
- S(n) = k S(n/k) + n

**Big O and Friends**
- We say 
	- $f(n) = O(g(n))$ if there exists constants c and $n_{o}$ such that $f(n) \leq cg(n) \quad \forall n>n_o$
	- $f(n) = \Omega(g(n))$ if there exists constant c and $n_{o}$ such that $f(n) \geq cg(n) \quad \forall n > n_{o}$

**Recurrence Relations**
- Finding closed form solutions
	- Keep going to until we reach the base case. 
- Example with logs: 
	- $T(n) = T\left( \frac{n}{2} \right) + 5, T(1) = 1$
	- $T\left( \frac{n}{4} \right) + 10$ 
	- ....
	- $T\left( \frac{n}{2^{k}} \right) +5k$
	- stop when $\frac{n}{2^{k}} =1$
	- $k = \log_{2}(n)$
	- $T(1) + 5log_{2}k$

**Merge Sort**
- ignoring the space used for arguments and space used for function call stack algorithm uses worst case O(1) additional memory. 
- Worst case space complexity is $\Theta(\log n)$
- A merge sort implemented with linked lists can be implemented recursively with less auxiliary memory. 

**Array Resizing**

Depends on implementation
*Adding a set amount of size* (+10 space)
- Total runtime is $\Theta(n^2)$
- The amortized time complexity per insert is $\Theta(n)$
- Worst case time complexity per insert is $\Theta(n)$

*Doubling the Size*
- Most inserts are $\Theta(1)$
- Amortized per insert is $\Theta(1)$
- Total running time of first n pushes is $\Theta(n)$


**Queues**
- with linked list place front of queue at head
- Place end of queue at the tail of the listf
- enqueue at the back, dequeue at the front. 

**Stacks**
- Look at order of elements for push/pop

**Stack and Queue Implementation**
- 2 stacks has O(n) enqueue dequeue implementation

**Queue Implementation**
- Insert at tail of this list when using linked list for enqueue. 
- Remove at head of list when using linked list.
- For array implementations, read what is available.

**Array Resizing**
- +n means n^2 
- \*n means n
- 