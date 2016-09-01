# Contains Duplicate

Given an array of integers, find if the array contains any duplicates. 

Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.




---

###Unordered Containers are Faster Than Their Cousins

Using set, the run time is 96ms.

WHile using unordered_set, the run time is shortened to 42ms.

```
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> uniqueNums;
        for(int i = 0; i < nums.size(); i++){
            if(uniqueNums.count(nums[i]) != 0){
                return true;
            }
            uniqueNums.insert(nums[i]);
        }
        return false;
    }
};
```
