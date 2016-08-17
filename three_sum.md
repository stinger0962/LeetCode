# Three Sum

Given an array S of n integers, are there elements a, b, c in S such that ```a + b + c = 0```? Find all unique triplets in the array which gives the sum of zero.

Note: The solution set must not contain duplicate triplets.

For example, given array S = ```[-1, 0, 1, 2, -1, -4]```,

A solution set is:
```
[
  [-1, 0, 1],
  [-1, -1, 2]
]```



### Double Pointers

A naive solution will take O($$N*N*N$$) run time. We **sort the array** first and adopt **double pointers** to achieve O($$N*N$$) run time.

We go through each number **in a main loop**, trying to **find two numbers** greater which will make the total sum zero. 

We use double pointers to represent such numbers, starting from the **left and right boundary**.

###Avoid Duplicate

Possible duplicate occurs when two adjcent numbers have the same value.

We separate this problem in two cases.

1. The repeated value occurs **in the main loop**. For example, ```nums[i]``` has the same value as ```nums[i-1]```. We find that the solution of ```nums[i]``` has already been included in that of ```nums[i-1]```. Thus, we can skip ```nums[i]```.

2. The repeating occurs in left or right pointers. Let's say we have left pointing to ```nums[k]```, and ```nums[k+1] == nums[k]```. We notice that two of the three numbers are same, which implies that the third one should also be same because their sum is zero. Thus, we skip nums[k+1]. 

**In the main loop, we skip a position if its value is same as the previous one.**

**In DP, we skip a position if next position has the same value as current one.**

###The Code

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        if(nums.empty() || nums.size() == 1){
            return result;
        }
        sort(nums.begin(), nums.end()); // Sort the vector in ascending order
        for(int i = 0; i < nums.size() - 2; i++){
            // Avoid duplicate, since all solutions of left/right of i are already included in that of i-1
            if(i > 0 && nums[i] == nums[i-1]){ 
                continue;
            }
            // Double pointers : start from left and right boundary, move towards each other
            int left = i + 1;
            int right = nums.size() - 1;
            while(left < right){
                int sum = nums[i] + nums[left] + nums[right];
                if(sum == 0){
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]}); // Solution found
                    // Avoid duplicate numbers from both sides
                    // Reason: if duplicate exists, then two of three numbers are the same as that of the previous solution,
                    // which implies a duplicate solution
                    while(left + 1 < right && nums[left] == nums[left+1]){
                        ++left;
                    }
                    while(right - 1 > left && nums[right] == nums[right-1]){
                        --right;
                    }
                    // Double pointers move towards each other
                    ++left;
                    --right;
                }
                // If sum < 0, then the left is too "negative", we must move it right
                else if(sum < 0){
                    // Avoid duplicate, reason is pretty much the same as above ( 2 of 3 numbers are same)
                    while(left + 1 < right && nums[left] == nums[left+1]){
                        ++left;
                    }
                    ++left;
                }
                else{
                    while(right - 1 > left && nums[right] == nums[right-1]){
                        --right;
                    }
                    --right;
                }
            }
        }
        return result;
    }
};
```




