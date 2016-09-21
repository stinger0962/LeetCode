# Remove Element


Given an array and a value, remove all instances of that value in place and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example:
Given input array ```nums = [3,2,2,3], val = 3```

Your function should return length = 2, with the first two elements of nums being 2.



Extra points : What happens when the elements to remove are rare?




---



###Two Solutions Using Double Pointers

First solution is very same to that of 9.9 Remove Duplicate from Sorted Array. 

We use a fast pointer to search for special value, while reconstruct the array using slow pointer.

For details, refer to* 9.9 Remove Duplicate from Sorted Array*.



> What happens when the elements to remove are rare?



In the first solution, we loop the list and do something when **num != value**.

The solution becomes inefficient when the elements to remove are rare. 

For example, We have array ```(1,2,3,4,5)``` and value = ```5```.

We are doing unnecessary copies for 1,2,3,4.

In the above case, an **efficient solution** is to do something when **num == value**



###The Code

*General Solution -- Reconstruct the Array*

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        if(nums.empty()) return 0;
        // Fast pointer j increments by 1 each time.
        // Slow pointer i increments by 1 when nums[j] stays, 
        // Meanwhile, we rebuild array using index i.
        int i = 0;
        for(int j = 0; j < nums.size(); j++){
            if(nums[j] != val){
                nums[i] = nums[j];
                i++;
            }
        }
        return i;
    }
};
```

*Special Solution -- When Elements to Remove Are Rare*

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        if(nums.empty()) return 0;
        // Fast pointer j increments by 1 each time.
        // Slow pointer i increments by 1 when nums[j] stays, 
        // Meanwhile, we rebuild array using index i.
        int n = nums.size();
        int i = 0;
        while(i < n){
            // When a target element is found, swap it with the last element
            // Then erase the last element, and reduce array size by 1
            if(nums[i] == val){
                nums[i] = nums[n-1];
                nums.erase(nums.begin() + n - 1);
                n--;
            }
            else{
                i++;
            }
        }
        return i;
    }
};
```