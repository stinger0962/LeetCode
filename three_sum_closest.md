# Three Sum Closest

Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

```
    For example, given array S = {-1 2 1 -4}, and target = 1.

    The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).```




---

###Extension of The Previous Three Sum
  This problem is an extension of the previous one -- *9.4 Three Sum*. 
  
  These two problems share the same strategy. Both sort the array first, then use a main loop, and an inner loop with double pointers. 
  
  The difference is that in this problem we calculate the sum each time we move around pointers, and update the closest sum as necessary. 
  
  **In the inner loop**, if we find the **sum is less** than the target, then the only choice is to **move left forward**; likewise, if the **sum is greater** than the target, we have to **move right backward**. This process repeats until two pointers cross each other.



###The Code

```
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        // Special cases
        if(nums.empty()){
            return 0;
        }
        if(nums.size() == 1){
            return nums[0];
        }
        if(nums.size() == 2){
            return nums[0] + nums[1];
        }
        sort(nums.begin(), nums.end());
        int closestSum = nums[0] + nums[1] + nums[2]; // Assign a default value to the closet sum
        for(int i = 0; i < nums.size() - 2; i++){
            int left = i + 1; // Similar to Three Sum I, we use double pointers
            int right = nums.size() - 1;
            // Until DP cross, if sum > target, move right backward; if sum < target, move left forward
            // Record sum and diff each time
            while(left < right){
                int sum = nums[i] + nums[left] + nums[right];
                if(sum == target){
                    return sum;
                }
                if(sum > target){
                    closestSum = abs(closestSum - target) < sum - target ? closestSum : sum; // Update closest sum when necessary
                    right--;
                }
                else{ // sum < target
                    closestSum = abs(closestSum - target) < target - sum ? closestSum : sum;
                    left++;
                }
            }
        }
        return closestSum;
    }
};
```