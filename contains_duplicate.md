# Contains Duplicate I

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




---


# Contains Duplicate II

Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the difference between i and j is at most k.



---


###The Algorithm

As always, we want to achieve whatever we want in a single traversal. 

The key point is for each element ```nums[i]```, we only look into its left k elements, to see if the subset contains ```nums[i]```. 


We maintain a dynamic set of K numbers just left to each element. 

If there are not enough numbers, then the set is the entire subset left to the element.

Duplicate is found if the element is contained in the set.


###The Code


```
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_set<int> subset;
        // For each element in the array, build a subset of [k](or as many as possible) elements left to it
        // Check whether the element appears in the range or not
        for(int i = 0; i < nums.size(); i++){
            if(i > k){
                subset.erase(nums[i-k-1]);
            }
            if(subset.find(nums[i]) != subset.end()){
                return true;
            }
            subset.insert(nums[i]);
            
        }
        return false;
    }
};
```