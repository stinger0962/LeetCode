# 1.1 Two Sum

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution.

### Example:
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1]

> UPDATE (2016/2/13):
The return format had been changed to zero-based indices. Please read the above updated description carefully.


   The key is to think differently. Normal thinking leads to think forward, comparing item i with i+1, i+2, ...
 A neat solution to this problem requires to think backward, that is, for item i, focus on i-1, i-2, ...
 
   As for the data structure, a C++ implementation of hashtable -- map with $$nlogn$$ time complixity is acceptable (24ms beaten 41% while the most common solution has a runtime of 16ms)


```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, int> mapNums;
        for(int i = 0; i < nums.size(); ++i)
        {
            int complement = target - nums[i];
            if(mapNums.find(complement) != mapNums.end()) // be aware that argument passed into find is key while not value
            {
                int index = mapNums.find(complement)->second;
                return vector<int>{index, i};
               
            }
            mapNums[nums[i]] = i; // be aware that key is the actual number stored in the original vector
        }
        throw invalid_argument("There is no solution");
    }
};```