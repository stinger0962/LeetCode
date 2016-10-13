# Tree

##Binary Index Tree/Fenwick Tree

BIT stores accumulative sums. We can retrieve the sum of first k elements, and update i-th element both in O(logN) time.

*2.21 Range Sum Query 2D - Mutable*

##Segment Tree

Segment tree store intervals. It is a flexible data structure. 

We can use it to retrieve a lot of things such as max, min, elements between range l-th and r-th in O(logN) time. 

We can also add quantity to i-th element, or to each element from l-th to r-th in O(logN) time.

Segment tree can simulate the behaviors of BIT. So why would you prefer a BIT over Segment Tree?

BIT is much easier to code, and requires less memory. So if BIT can solve the problem, use it; otherwise use Segment Tree.



---


*2.20 Range Sum Query - Mutable*

