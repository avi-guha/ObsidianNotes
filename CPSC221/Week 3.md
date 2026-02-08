# Linked List
## Type Compatibility for Pointers

- each \* removes a '\*'
- each & adds a \*
Working with pointers etc. 
``` c++;
// Online C++ compiler to run C++ program online
#include <iostream>
using namespace std; 

int main() {
    // Write C++ code here
    int x = 23; 
    cout << x << endl;
    
    int *p = &x;
    cout << p << endl; 
    cout << *p << endl; 
    
    int **q = &p; 
    cout << q << endl;
    cout << *q << endl; 
    cout << **q << endl; 
}
```
Output
```
23
0x7ffc1d7fe584
23
0x7ffc1d7fe578
0x7ffc1d7fe584
23
```
- different data types occupy different amounts of memory 
- The pointer data type tells compiler how many bytes must be retrieved when dereferencing the pointer.

## Pointers and Dynamic Memory 
- The new keyword allocates space in dynamic memory and returns the first address of the allocated space. 
- delete releases the memory at the address referenced by its pointer variable. 
	- delete[] is used to release memory allocated to array variables. 

Incorrect code example: 
``` c++;
#include <iostream>
using namespace std; 
int main() {
    int a = 5; 
    int *b = new int; 
    int *c = &a; 
    *c = 4; 
    int **d = &b;
    
    int *e = new int[a]; // allocates an array of size a in dynamic memory (heap)
    int *f = new int[*b];
    delete b; 
    delete e;
    delete[] f; 
}
```
Fixed: 
``` c++;
#include <iostream>
using namespace std; 
int main() {
    int a = 5; 
    int *b = new int(a); 
    int *c = &a; 
    *c = 4; 
    int **d = &b;
    
    int *e = new int[a]; // allocates an array of size a in dynamic memory (heap)
    int *f = new int[*b];
    delete b; 
    delete[] e;
    delete[] f; 
}
```

## Dangling Pointers 
- When we are done with an allocated object, we free it so that the system can reclaim and later reuse the memory. 
- This is also known as use after free. 
- If the pointer continues to refer to the deallocated memory, it will behave unpredictably when dereferenced and (and memory is reallocated) - dangling pointer 
	- Leads to bugs that can be subtle and difficult to find
	- Set the pointer to null after freeing
``` c++;
// Online C++ compiler to run C++ program online
#include <iostream>
using namespace std; 
int main() {
    int *i = new int; 
    *i=5; 
    cout << *i << endl;
    delete i; 
    i = nullptr; 
    cout << *i << endl; // causes seg fault, accessing freed memory. 
}
```

## Memory Leaks 
- If you lose access to allocated space (by reassigning a pointer), the space can no longer be referenced or freed. 
	- This remains marked as allocated for the lifetime of the program. 
- Example of code that does this
``` c;
int main() {
    int *arr; 
    int sz = 4; 
    arr = new int[sz];
    arr[2] = 5; 
    arr = new int[sz];
    arr[2] = 7; 
}
```

## Linked List Nodes 
- A dynamic data structure that consists of nodes linked together. 
- A node is a data structure that contains
	- data 
	- location of the next node. 

## Node Pointers 
- A node contains the address of the next node in the list 
	- In C++ this is recorded as a pointer to a node
- Nodes are created in dynamic memory.  (one at a time using new keyword).
	- Their memory locations are not in sequence. 
- The data attribute of a node varies depending on what the node is intended to store. 

## Linked Lists 
- A linked list is a chain of nodes where each node stores the access of the next node. 
![[Pasted image 20260131203043.png]]

## Linked List Implementation 

``` c++;
template <class LIT>
struct Node {
LIT data; 
Node * next; 
Node(LIT ndata, Node * nx = NULL):data(ndata), next(nx) {}
};
```
- attributes / members of a particular node can be accessed using the '.' operator. 
	- or the -> operator is shorthand for pointer types.
	- This is the equivalent of dereference, then access. 
## Building a Linked List 
``` c; 
Node<int>* a = new Node<int>(7, null);
```
![[Pasted image 20260131203946.png]]
``` cpp;
Node<int>* a = new Node<int>(7, null);
a->next = new Node<int>(3, null);
```

### Traversing a Linked list 
``` cpp;
Node<int>* a = new Node<int>(7, null);
a->next = new Node<int>(3, null);
Node<int>* p = a;
p = p->next; // go to next node
p = p->next;
```

Another we do this is the following 

```c++;
Node<int>* p=a; 
while(p! = nullptr){
//process p
p = p -> next; 
}
// p is NULL when leaving loop
```
- Note that we do not use ``` a = a->next ```
	- A is a stack pointer to the first node, if change it, we have lost the memory. 
	- Cost of traversal for a list of length n is O(n)

## Linked List Insertion 
- Insertion in a singly linked list requires only updating the next node reference of the preceding position. 
- ![[Pasted image 20260131210234.png]]
``` c;
Node<int>* b = new Node<int>(17, p->next);
p->next = b;
```
- We must be aware that sequential nodes are not guaranteed to be found in sequential memory locations.

## Linked List Removal 
- Likewise, we can remove a node by updating the pointer of preceding node. 
	- Also remove to deleted node. 
``` c++;
p->next = b->next;
delete b;
```
- getting 'b' and 'b' in position requires traversal, O(n).
- Complexity of gaining access to last node in list? O(n) 
	- how to get p, if only have b
	- can't from b must traverse from a. 
![[Pasted image 20260131210656.png]]

# Linked List Continued 

## Linked lists, by features. 

- **Linkage**
	- Singly-linked 
		- nodes have pointer to next node 
	- Doubly-linked 
		- nodes have pointer to next and previous node. 
* **Termination**
	* null-termination
		* next pointer of last node is null. 
* **Access / Entry**
	* Front/head pointer gives access to the first node in the list 
	* Front + back pointers 

## Linked List Variations 
**Node**
``` c; 
template <class LIT>
struct Node {
LIT data;
Node* next;
Node(LIT ndata, Node * nx=NULL):
data(ndata),next(nx) {}
};

```

**Linked List**

```c;
Template <class LIT>
class LinkedList {
	private:
	Node* head;
	Node* tail;
	int length;
	public:
	LinkedList();
...
};
```

- Suppose we have a basic singly linked list with a head pointer as defined above 
	- operations at the back of the list have relatively poor complexity (requires traversal)
	- We can give ourselves a tail pointer for very little overhead, maintained during operations at the back of the list. 

## Singly-linked list with a tail pointer 
- useful for insertion at the back of the list. 
- ![[Pasted image 20260131211923.png]]
- This gives us ability to perform insertion at the back of the list in O(1) time. 
	- Insertion in the middle of the list is still O(n)
	- removal from back of the list is still O(n) in singly linked lists because there is no back pointer from tail to previous node. 

## Doubly - Linked List 
- Node definition contains an additional pointer 
``` c; 
template <class LIT>
struct Node {
	int data;
	Node* prev;
	Node* next;
// constructors, etc.
};
```
- this provides access to the previous nodes and next nodes from a single pointer 
	- requires more pointer management in programming. 
![[Pasted image 20260131212709.png]]


## Doubly-linked list insertion 
- After some specified node 

```c; 
Node<int>* curr, * temp;
... // use a loop to move curr into place
temp = new Node<int>();
temp->data = 7;
temp->prev = curr;
temp->next = curr->next;
curr->next->prev = temp;
curr->next = temp;
```

we can also do the following: 
``` c
Node * curr_n = curr -> next; 
curr -> next = temp; 
temp -> prev = curr; 
temp -> next = curr_n; 
curr_n ->prev = temp; 
```

![[Pasted image 20260131212909.png]]
- curr_n is 4. 

## Doubly-linked list removal 
- at some specified node 
``` c;
Node<int>* curr;
... // move curr to the node to be removed
curr->next->prev = curr->prev;
curr->prev->next = curr->next;
delete curr;
curr = NULL;
```
![[Pasted image 20260131213008.png]]

## Linked List Variations 
- Circularly singly linked list 
	- ![[Pasted image 20260131213132.png]]
- Circular doubly-inked list 
	- ![[Pasted image 20260131213148.png]]
- Sentinel Nodes  
	- "dummy" nodes at the ends of lists which do not contain any data. 
	- eliminate special cases for list modifications 
		- insert/remove first/last node, all cases implemented the same way. 
![[Pasted image 20260131213306.png]]

## Recursion in Linked Lists 
- Iteration is convenient for many list operations 
	- Can be costly for other operations 
- For example printing the contents of a single 
- ![[Pasted image 20260131213532.png]]
- if we use iteration this is $O(n + n-1 + n-2 + ... 2 + 1) = O(n^2)$
- We can use recursion to do this much faster. 
``` c; 
template <class LIT> 
void PrintReverse(Node<LIT> *curr){
		if(curr != nullptr){
		PrintReverse(curr->next);
		cout << curr -> data << " ";
	
	} // implicit base case: do nothing if list empty 
} 
```
- We have access to the current node and the rest of the list. 
- We must trust the natural recursion. 
- If we ask recursion to print the rest of the list in reverse, where do we print our current node relative to that? 
- Running time? 
	- $T(N) = T(n-1) + C$
		- This shows us the cost of printing the rest of the list + cost to print current node. 
		- This is $O(n)$

Test: Reversing a linked list. 
``` c;
Node* reverseLinkedList(Node* head) {

    Node* prev = nullptr;

    Node* current = head;

    Node* next = nullptr;

  

    while (current != nullptr) {

        next = current->next; // Store next node

        current->next = prev; // Reverse current node's pointer

        prev = current;       // Move pointers one position ahead

        current = next;

    }

    return prev; // New head of the reversed list

}
```