https://www.youtube.com/watch?v=oBt53YbR9Kk

A way of thinking to solve certain problems. For DP to work, 
- we should be able to explain a problem in terms of its sub problems
- we should identify base cases and know its outcome beforehand

For example, in case of Fibonacci sequence:

1. we know that `fib(n)` is dependant on `fib(n-1)` and `fib(n-2)` simply by the way fibonacci sequence is described.
2. A "brute force" recursive solution would look something like this:

```python

def fib(n):
	if n == 1 or n == 2:
		return 1
	
	return fib(n-1) + fib(n-2)
```

so solution to a particular problem is comprised of sub solutions and if the corresponding sub solutions are solved, we can assume that the original problem is solved. That's the logic. In recursion we go deeper and deeper until we hit a base case.

The above solution is far from efficient. To understand the efficiency of a solution, we can visualize it.

![[fib_recursive.jpg]]

As we can see here, for a given `n` the total number of nodes in the bottom rank, which is where the base cases are, is `2**n`. So the time complexity would be `O(2**n)` and the space complexity would be `O(n)`. why? because in the memory stack, starting from the fib(7) node, the calls go deeper and deeper till a base node is hit, after that the base node is removed from the stack before a new node is added, this way, the total memory needed is just `n` spaces.

How to better this solution?

The easiest answer is memoization. We can see in the above tree that in a recursive solution, some subtrees are recalculated, we can avoid this if we save the solutions of subproblems we know in a cache.

After applying memoization, the tree should look like this

![[fib_memoize.jpg]]

As you can see here, as we go down the tree in one of the branches, we obtain solutions for all the subproblems we need to solve any part of the recursive tree. In this way we have reduced the complexity. But by how much?

new time complexity: `O(n)` (technically 2n, but it is simplified to n) 
new space complexity: `O(n)` (again 2n)

Tabulation:

Other than memoization, which works close to how the problem is defined and works top down, we have another DP technique, called `tabulation`. Which is to start from a base case and work up the solutions until the given conditions are met.

For example, with fibonacci sequence, we know some facts like the 0th element is 0 and the first el is 1. we can use this to work up to find any `fib(n)`

```python
def fib(n):
    table = [0]*(n+2)
    table[1] = 1
    
    for i in range(n):
        table[i+1] += table[i]
        table[i+2] += table[i]
    return table[n]
```

While this isn't a very pythonic solution, technically the recipie is to allocate an array of length n upfront, so that each index is allocated for a solution of a sub problem and when we have enough sub problem solved, solving a super problem becomes possible.

Tabulation recipie:
- Visualize the problem as a table
- size the table based on the inputs
- initialize the table with a sensible default values
- seed the trivial answer into the table
- iterate through the table
- fill further positions of the table based on current position

