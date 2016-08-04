# 9.2 Minimum Size Subarray


Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there isn't one, return 0 instead.

For example, given the array ```[2,3,1,2,4,3]``` and ```s = 7```,
the subarray ```[4,3]``` has the minimal length under the problem constraint.



---



### Read From Right to Left 

Like *9.1 the longest substring without repeating characters*, the key is to **read a subarray from right to left**. We loop through the array and find the required subarray left to each element.

### The O(N) Algorithm

1. We use double pointers to solve the problem. Subarray bounded by the two pointers is the one we need to find.

2. While the right pointer loops through the array, keep adding elements to a variable sum. 

3. If sum meet the requirement, start from the left pointer, remove as many elements as possible, so that sum still meets the need.

4. At this time, we get the min length of subarray which ends at the right pointer. 

5. Make update to min length as necessary and move the right pointer ahead. 


### The Code

```
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) 
    {
        if(nums.size() == 0)
        {
            return 0;
        }
        int minLen = INT_MAX; // INT_MAX is good to find non-zero min
        int sum = 0; 
        for(int i = 0, j = 0; i < nums.size(); i++) // Double pointers, i:right, j: left
        {
            sum += nums[i];
            while(sum >= s) // Remove as many elements at the beginning as possible to find the min length of i
            {
                minLen = min(minLen, i-j+1);
                sum -= nums[j];
                j++;
            }
        }
        return minLen == INT_MAX ? 0 : minLen;
    }
};
```
