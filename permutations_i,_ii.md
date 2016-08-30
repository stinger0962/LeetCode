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

We can still find an available solution by introducing a trick that passes the array **by value** and **does not swap numbers back**. 

By passing the array by value, the swapped numbers only persist in a single route from outmost to inner most loop. The swap doesn't effect parallel recursion calls. (This part is hard to understand!)

```
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> solution;
        sort(nums.begin(), nums.end());
        permuteBacktrack(solution, nums, 0);
        return solution;
    }
private:
    void permuteBacktrack(vector<vector<int>>& solution, vector<int> nums, int start){
        if(start == nums.size()-1){
            solution.push_back(nums);
        }
        for(int i = start; i < nums.size(); i++){
            if(i != start && nums[i] == nums[start] ){
                continue;
            }
            swap(nums[i], nums[start]);
            permuteBacktrack(solution, nums, start+1);
            
        }
    }
};
```
However, passing by value is not straightforward or easy to understand. We will also try a different appraoch.

###Easy to Understand Depth-First-Search Solution

The idea is to use an extra array of boolean value to store whether a number is already added into the set or not.

**Pros** is that this solution is straightforward and easy to follow. Plus, this solution is also suitable to solve permutation without repetitions. 

**Cons** is that it does use extra space to store boolean values for each loop.


```
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> solution;
        vector<int> route;
        vector<bool> used(nums.size(), false);
        sort(nums.begin(), nums.end());
        permuteBacktrack(solution, route, nums, used);
        return solution;
    }
private:
    void permuteBacktrack(vector<vector<int>>& solution, vector<int>& route, vector<int>& nums, vector<bool> used){
        if(route.size() == nums.size()){
            solution.push_back(route);
            return;
        }
        for(int i = 0; i < nums.size(); i++){
            if(used[i]){
                continue;
            }
            // We skip a number if it is duplicate with its previous, 
            // and the previous one is not used yet
            // Omit this condition, it's also a solution for permutation w/o duplicate
            if(i != 0 && nums[i] == nums[i-1] && used[i-1] == false){
                continue;
            }
            used[i] = true;
            route.push_back(nums[i]);
            permuteBacktrack(solution, route, nums, used);
            used[i] = false;
            route.pop_back();
        }
    }
};
```

