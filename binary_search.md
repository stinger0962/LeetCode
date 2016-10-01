# Binary Search

Binary search is an algorithm to search an array of length n in $$O(logn)$$ time complexity.

The algorithm requires a **sorted** array.

We use two variables to record the start index and ending index.

Each time we compare the middle point with target, then eliminate half of the range according to the result.

The algorithm finishes when two indexes equal to each other.

###Relation to Recursion

Binary search is **not bound** to recursion. 

Although many cases involve using binary search together with recursion, there are times that a loop will be sufficient.

(e.g.) If we only want to search for the index, we can use a **while** loop, each step of which both indexes move towards each other.



