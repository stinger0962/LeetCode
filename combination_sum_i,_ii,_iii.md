# Combination Sum I, II, III


> This article is about a series of three problems.

## Combination Sum I

Given a **set** of candidate numbers (**C**) and a target number (**T**), find all unique combinations in **C** where the candidate numbers sums to **T**.

The **same** repeated number may be chosen from **C** unlimited number of times.

Note:
All numbers (including target) will be **positive** integers.
The solution set must not contain duplicate combinations.
For example, given candidate set``` [2, 3, 6, 7]``` and target ```7```, 
A solution set is: 
```
[
  [7],
  [2, 2, 3]
]```


**Note** : The SET of candidate numbers implies that there are NO duplicate candidate numbers.

---


### Find All Solutions, Try Backtracking

This is a typical find-all-solutions problem. We build the solution set from all possible routes and abandon a route when it becomes invalid. 

###When to abandon?

We notice that all numbers, including the target are **positive** numbers. Such restriction allows us to add up all the candidates as we build the solution, and abondon the last candidate(backtrack) when the sum exceeds the target.

###Avoid duplicate solution

To avoid duplicate solutions, we need to introduce an additional parameter which records the starting position of each loop. The starting index **ONLY** moves forward.


###The Code

```
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> solution;
        vector<int> route;
        combinationSumBacktrack(solution, route, candidates, target, 0);
        return solution;
    }
private:
    void combinationSumBacktrack(vector<vector<int>>& solution, vector<int>& route, vector<int>& nums, int target, int start){
        if(target < 0){
            return;
        }
        if(target == 0){
            solution.push_back(route);
            return;
        }
        for(int i = start; i < nums.size(); i++){
            route.push_back(nums[i]);
            combinationSumBacktrack(solution, route, nums, target - nums[i], i);
            route.pop_back();
        }
    }
};
```


## Combination Sum II

Given a **collection** of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

Each number in C may only be used **once** in the combination.

Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
For example, given candidate set [10, 1, 2, 7, 6, 1, 5] and target 8, 
A solution set is: 
```
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]```


---


###Difference.

1. Duplicate numbers exist in the collections of candidates.
2. Each number may only be used once in the combination.

We make appropriate changes, respectively.
1. Sort the array. Then in the main loop, we skip a number which is equal to the preceding one.
2. In the recursive call, we start from i+1 while not i.

###The Code

```
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<vector<int>> solution;
        vector<int> route;
        combinationSumBacktrack(solution, route, candidates, target, 0);
        return solution;
    }
    
private:
    void combinationSumBacktrack(vector<vector<int>>& solution, vector<int>& route, vector<int>& nums, int target, int start){
        if(target < 0){
            return;
        }
        if(target == 0){
            solution.push_back(route);
            return;
        }
        for(int i = start; i < nums.size(); i++){
            if(i == start || nums[i] != nums[i-1]){
                route.push_back(nums[i]);
                combinationSumBacktrack(solution, route, nums, target - nums[i], i+1);
                route.pop_back();
            }
        }
    }
};
```


## Combination Sum III

Find all possible combinations of k numbers that add up to a number n, given that only numbers from **1 to 9** can be used and each combination should be **a unique set of numbers**.


Example 1:

Input: k = 3, n = 7

Output:

```
[[1,2,4]]
```

Example 2:

Input: k = 3, n = 9

Output:

```
[[1,2,6], [1,3,5], [2,3,4]]
```

### The difference

1. We must use exact k numbers to construct the sum. 
2. The candidates come from a fixed unique number set: 1,2,...,9.

We make appropriate changes, respectively.
1. Omit a parameter which indicates the number set, add a new one denoting the number of candidates.
2. Introduce a new ending condition where the number of candidates is used up.


###The Code

```
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> solution;
        vector<int> route;
        combinationSumBacktrack(solution, route, k, n, 1);
        return solution;
    }
private:
    // k: how many numbers still needed
    // target: the target sum
    // start: the starting number to begin a loop. It is between 1 to 9
    void combinationSumBacktrack(vector<vector<int>>& solution, vector<int>& route, int k, int target, int start){
        // Notice that order of the following two if statements matters
        if(target == 0 && k == 0){
            solution.push_back(route);
            return;
        }
        if(target < 0 || k == 0){
            return;
        }
        for(int i = start; i < 10 ; i++){
            route.push_back(i);
            combinationSumBacktrack(solution, route, k - 1, target - i, i + 1);
            route.pop_back();
        }
    }
};
```



