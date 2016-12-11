Given a sorted array of integers, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order ofO\(logn\).

If the target is not found in the array, return`[-1, -1]`.

For example,  
Given`[5, 7, 7, 8, 8, 10]`and target value 8,  
return`[3, 4]`.





---



## Typical Binary Search



```
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int sz = nums.size();
        vector<int> result;
        int small = INT_MAX;
        int large = INT_MIN;
        searchRangeRecur(nums, small, large, 0, sz, target);
        if (small != INT_MAX) {
            result.push_back(small);
            result.push_back(large);
        }
        else {
            result.push_back(-1);
            result.push_back(-1);
        }
        return result;
    }
    
private:
    // search target between [nums[lo], nums[hi]), add index to range if found
    void searchRangeRecur(vector<int>& nums, int& small, int& large, int lo, int hi, int tar) {
        if (lo > hi - 1) return;
        else if (lo == hi - 1) { // searching range only contains one last element
            if (nums[lo] == tar) {
                small = lo < small ? lo : small;
                large = lo > large ? lo : large;
            }
            return;
        }
        else {
            int mid = lo + (hi - lo) / 2;
            if (nums[mid] < tar)
                searchRangeRecur(nums, small, large, mid+1, hi, tar);
            else if (nums[mid] > tar)
                searchRangeRecur(nums, small, large, lo, mid, tar);
            else {
                small = mid < small ? mid : small;
                large = mid > large ? mid : large;
                searchRangeRecur(nums, small, large, lo, mid, tar);
                searchRangeRecur(nums, small, large, mid+1, hi, tar);
            }
        }
        
    }
};
```



