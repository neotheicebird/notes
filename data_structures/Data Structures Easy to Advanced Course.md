https://www.youtube.com/watch?v=RBSGKlAvoiM

Data Structures (DS) - A way of arranging data such that the data can be used effectively. Leads to faster execution, simpler-cleaner code and opens up possibilities to expand any software

Abstract Data Type (ADT) - Abstraction of a data structure which provides only the interface to which the data structure must adhere to, without dictating the details of implementation

ADT: Implementation - 1. List: (Dynamic Array, Linked List), 2. Queue: (Array based queue, Stack based queue) 3. Map: (Tree Map, Hash Map)

Big-O: Quantifying the time and space complexity for the "worst" case, for an algorithm. There are other measurements like Big-Omega, Big-Thetha which measures other things about an algorithm. Below is the list of common complexities of algos from small to large complexities:

Constant Time: O(1)
Logarithmic Time: O(log(n))
Linear Time: O(n)
Linearithmic Time: O(nlog(n))
Quadric Time: O(n^2)
Cubic Time: O(n^3)
Exponential Time: O(b^n) where b > 1
Factorial Time: O(n!)

Let f be a fn that describes the running time of an algo of input size n:

f(n) = 7log(n) ^ 3 + 15n^2 + 2n^3 + 8

O(f(n)) = O(n^3)

as cubic is the most complex part of the algo, we ignore the others.

Constant Time Example:

```
# example 1
a = 1
b = 2
c = a + 5*b

# example 2
i = 0
while i < 11:
    i += 1
```

Even with a loop the 2nd example is constant time, because even if the input size becomes arbitarily large, the execution time wouldn't change.

Linear time example:

```
i = 0
while i < n:
    i += 3
```

Even though we would only run approx. n/3 iterations, still the algo is considered O(n) because as n increases the number of iterations is directly proportional to n.

Basically we are estimating the relationship between the input and the time taken for execution.

Quadratic Example:

```
for i in range(n):
    for j in range(n):
	    # do something
```

Logarithmic Example (binary search):

```
# suppose we have a sorted array and we want to find the index of `el`

low = 0
high = n-1
while low <= high:
    mid = (low + high) // 2
	
	if array[mid] == value: return mid
	elif array[mid] < value: low = mid + 1
	elif array[mid] > value: high = mid - 1

return -1
```

Arrays: A fundamental data structure, a building block that can be used to create almost every other data structure.

Static Array: A fixed length container containing n elements which are indexable from range [0, n-1]

What is meant by being "indexable"?

Each slot/index in the array can be referenced with a number. A chunk of memory is contiguous of the addresses, and a static array will be placed in such a chunk, making the indexing efficient.

Examples of static array: Buffer to store small chunks of file, As lookup tables and inverse lookup tables, return multiple values from a function, Used in dynamic programming to cache subproblems in tabulation.

Complexity:
                Static Array, Dynamic Array
Access: O(1), O(1)
Search: O(n), O(n)
Insertion: N/A, O(n)
Appending: N/A, O(1)
Deleting: N/A, O(n)

Dynamic Array: Array with insert, append capabilities. Dynamic arrays are made using static arrays. Say we first allocate a static array of size 2, and as elements are appended, the static array is used, until the 3rd element is added, in which case the a new static array of size double of previous array is made and the elements are copied to the new array (more or less)

## Singly and Doubly linked lists

  A sequential list of nodes that hold data which point to other nodes also containing data
  
  Data -> Data -> Data -> Data -> null
  
  - Used in many list, queue and stack implementations
  
  - Great for creating circular lists

Linked List terms: Head (first node), Tail (last node), Pointer (reference to another node), Node (an object containing data and pointer(s)).

Singly LL nodes only have pointers to next nodes. Doubly LL nodes have pointers to next and previous nodes.

SLL: Uses less memory, but cannot access previous element (we have to traverse from head again to get a previous node)

DLL: Can be traversed backwards, but takes 2x memory

### Insert in Singly Linked Lists

![[insert_sll.jpg]]

- To insert node after a particular node (current node), traverse the LL, find the interested node (23 in this case)
- Create the new node to be inserted and assign the next pointer to point to the reference the current node was pointing to
- Change the next pointer of the current node to point to the new node. With this we have successfully inserted the new node.

DLL insersion is similar to SLL insersion

### Remove in Singly Linked Lists

![[remove_sll.jpg]]

- To remove node after a particular node (current node), traverse the LL to get the current node, use another traverser which is one behind to get the previous node
- When you find a node of interest, assign the node to another pointer called temp, so that we can deallocate the memory later
- Now traverse only the current node traverser (trav2) by one node
- Set trav1's next pointer to trav2's node
- Remove the temp pointer and reallocate memory to avoid memory leak.

For DLL, we only need one traverser, so its actually easier to remove.

Complexity
               SLL vs DLL
Search: O(n), O(n)
Insert at head: O(1), O(1)
Insert at tail: O(1), O(1)
Remove at head: O(1), O(1)
Remove at tail: O(n), O(1)
Remove in middle: O(n), O(n)

