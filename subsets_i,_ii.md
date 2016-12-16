# Subsets I, II

## Subset I

Given a set of distinct integers, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If nums = [1,2,3], a solution is:

```
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

---


###Same Structure as Combination Sum 

This problem has a similar structure as the *combination sum series*. Difference is that there is no need to filter candidates, all fo which are valid solutions.

Such difference brings two changes in the backtracking function.

1. There is no early return.
2. There is no validation.


###The Code

```
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> solution;
        vector<int> subset;
        subsetsBacktrack(solution, subset, nums, 0);
        return solution;
    }
private:
    void subsetsBacktrack(vector<vector<int>>& solution, vector<int>& subset, vector<int>& nums, int start){
            // There are no if statements here.
              solution.push_back(subset);
              for(int i = start; i < nums.size(); i++){
                  subset.push_back(nums[i]);
                  subsetsBacktrack(solution, subset, nums, i+1);
                  subset.pop_back();
              }
          }
};
```


## Subset II

Given a collection of integers that might contain **duplicates**, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If **nums** = ```[1,2,2]```, a solution is:

```
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```



---

### Skip Duplicates

The process is same as that of Combination Sum II.

###The Code

```
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> solution;
        vector<int> subset;
        subsetsWithDupBacktrack(solution, subset, nums, 0);
        return solution;
    }
private:
    void subsetsWithDupBacktrack(vector<vector<int>>& solution, vector<int>& subset, vector<int>& nums, int start){
            // There are no if statements here.
            solution.push_back(subset);
            for(int i = start; i < nums.size(); i++){
                if(i == start || nums[i] != nums[i-1]){
                    subset.push_back(nums[i]);
                    subsetsWithDupBacktrack(solution, subset, nums, i+1);
                    subset.pop_back();
                }
                
            }
         }
};


```