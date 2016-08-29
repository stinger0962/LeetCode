# Permutations I, II

## Permutations I


Given a collection of distinct numbers, return all possible permutations.

For example,
```[1,2,3]``` have the following permutations:
```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]```


---


> This is a backtracking problem without early returning conditions.




###Algorithm for Generating All Permutation

There is a decent online tutorial about this topic.

[http://forum.codecall.net/topic/64715-how-to-generate-all-permutations/](http://forum.codecall.net/topic/64715-how-to-generate-all-permutations/)


The basic idea is to think of permuting (A1, A2, ... An) as the combination of

```
A1 + permute(A2, ...An), where A1 is at position 0
A2 + permute(A1, A3, ...An), where A2 is at position 0
...
An + permute(A1, A2, ... A(n-1) ), where An is at position 0```

Then we can recursively call the function on the array of n-1 elements. 

Yet, we still face a problem that the n-1 element array is dynamic.

To tackle the problem elegantly, we swap two elements each time we enter the loop, and swap them back  after the recursion returns.

```
For example. 
In the entry of A2 + permute(A1, A3, ...An) 
we first swap A1 and A2, 
then call the recursion on updated array(A2,A1,...An) 
                ^
Recursion will also need a parameter to indicate the starting index, 1 in the above case
when the recursion returns, we swap A1, A2 back.```


*Base case*

Starting from outmost loop, we have N swaps, then N-1 swaps, then N-2, until 1 last swap in the inner most loop. 

The base case is when we reach the inner most loop, at which time all elements have been swapped (except the last one, which will swap with itself, so it can be skipped).

There are N! different ways to reach the inner most loop, thus the run time complexity is N!.

###The Code

```
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> solution;
        permuteBacktrack(solution, nums, 0);
        return solution;
    }
    
private:
    // By swaping two elements, we omit the use of an extra vector to store the numbers
    void permuteBacktrack(vector<vector<int>>& solution, vector<int>& nums, int start){
        if(start == nums.size() - 1 ){
            solution.push_back(nums);
        }
        for(int i = start; i < nums.size(); i++){
            
            swap(nums[i], nums[start]);
            permuteBacktrack(solution, nums, start+1);
            swap(nums[i], nums[start]);
        }
    }
};
```


## Permutations II

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

For example,
```[1,1,2]``` have the following unique permutations:
```
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]```



---

###Not a Backtracking One

So far, we have dealt with duplicate in a single way. Sort the array first, then skip duplicate numbers in the loop. 

However, if we proceed this problem using the same approach, we will encounter a serious BUG. Due to swapping elements and passing by reference, the array will **NOT** keep sorted all the way along recursion.

By adding a trick which passes the array by value and doesn't swap back, we can still find a solution using swap. However, passing by value is not space efficient. So let's try a different appraoch.

###Next Permutation