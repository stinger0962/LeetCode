# 9.2 Minimum Size Subarray


Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there isn't one, return 0 instead.

For example, given the array ```[2,3,1,2,4,3]``` and ```s = 7```,
the subarray ```[4,3]``` has the minimal length under the problem constraint.



---



### Read From Right to Left 

Like *9.1 the longest substring without repeating characters*, the key is to find the min length left to each element of the array.

### The O(N) Algorithm

We use double pointers to solve the problem.

While the right pointer loops through the array, keep adding elements to a temp sum. 

If the sum meets the requirement, 

