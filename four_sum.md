# Four Sum

Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note: The solution set must not contain duplicate quadruplets.

For example, given array S = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
```
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]```



---


### Variation of Three Sum

Review *9.4 Three Sum* first to get the basic idea.


Implementation of four sum is almost as same as that of three sum.

In three sums, we use a main loop plus double pointers to evaluate every possible sum of 3 numbers.

In four sums, we use **double loops plus double pointers** to achieve the same goal.

To optimizing our code, we will consider extreming conditions and **pruning** at the beginning of each loop.


###The Code

```
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        int n = nums.size();
        vector<vector<int>> solution;
        if(n < 4){
            return solution;
        }
        sort(nums.begin(), nums.end());
        // The structure of main loop is similar to that of three sums,
        // except that we use double loops and omit unnecessary loops at the beginning
        for(int i = 0; i < n - 3; i++){
            // Omit duplicates
            if(i > 0 && nums[i] == nums[i-1]) continue;
            // No solution starting from index i, or any number greater than i
            if(nums[i] + nums[i+1] + nums[i+2] + nums[i+3] > target) break;
            // No solution starting from index i, solution possible starting from index i+1
            if(nums[i] + nums[n-3] + nums[n-2] + nums[n-1] < target) continue;
            for(int j = i + 1; j < n - 2; j++){
                if(j > i+1 && nums[j] == nums[j-1]) continue;
                if(nums[i] + nums[j] + nums[j+1] + nums[j+2] > target) break;
                if(nums[i] + nums[j] + nums[n-2] + nums[n-1] < target) continue;
                // KEY of the solution : Double pointers
                int left = j+1;
                int right = n-1;
                while(left < right){
                    int sum = nums[i] + nums[j] + nums[left] + nums[right];
                    if(sum == target){
                        solution.push_back(vector<int>{nums[i], nums[j], nums[left], nums[right]});
                        // Left and right will move towards each other at least once,
                        // Pointers will move multiple times if next number is found duplicate
                        do{
                            left++;
                        }while(left < right && nums[left]==nums[left-1]);
                        do{
                            right--;
                        }while(left < right && nums[right]==nums[right+1]);
                    }
                    if(sum < target){
                        do{
                            left++;
                        }while(left < right && nums[left]==nums[left-1]);
                    }
                    if(sum > target){
                        do{
                            right--;
                        }while(left < right && nums[right]==nums[right+1]);
                    }
                }
            }
        }
        return solution;
    }
};
```

