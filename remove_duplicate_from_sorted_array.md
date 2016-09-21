# Remove Duplicate from Sorted Array

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array ```nums = [1,1,2]```,

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.



---


###Using Double Pointers

We will use a pair of **fast and slow pointers** in both approaches.

One solution solves the problems by deleting duplicate numbers, while the other rebuilds non duplicate elements.

###The Code

*Erase duplicate elements*

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size() < 2) return nums.size();
        int newSize = nums.size();  // Return value
        int left = 0;
        int right = 1;
        while(right < nums.size()){
            if(nums[left] == nums[right]){
                nums.erase(nums.begin() + right);
                newSize--;
            }
            else{
                left = right;
                right++;
            }
        }
        return newSize;
    }
};
```

Rebuild a Non-duplicate Array

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.empty()) return 0;
        int size = nums.size();
        int i = 0;
        // Fast pointer j increments by 1 each time
        // Slow pointer i only moves forward when a number is not duplicate to the previous one
        // We also rebuild the array following index i.
        for(int j = 1; j < size; j++){
            if(nums[j] != nums[j-1]){
                i++;
                nums[i] = nums[j];
            }
        }
        return i + 1;
    }
};
```