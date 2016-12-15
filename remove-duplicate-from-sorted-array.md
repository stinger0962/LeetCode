Follow up for "Remove Duplicates":  
What if duplicates are allowed at mosttwice?

For example,  
Given sorted arraynums=`[1,1,1,2,2,3]`,

Your function should return length =`5`, with the first five elements ofnumsbeing`1`,`1`,`2`,`2`and`3`. It doesn't matter what you leave beyond the new length.



---



##Double Pointers


We use double pointers, one fast and one slow. Fast pointer constantly move ahead and check duplicate, while slow one move ahead and reconstruct the array numbers.


```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.empty()) return 0;
        int m = nums.size();
        int slow = 0; // slow ptr only moves ahead when certain conditions meet
        int fast = 0; // fast ptr moves ahead at constant speed
        while (fast < m - 1) {
            ++fast; // fast ptr move ahead
            bool dup = false;
            if (nums[fast] == nums[slow]) dup = true; // if duplicate found 
            while (nums[fast] == nums[slow] && fast < m - 1) ++fast; // move fast ptr ahead until non-duplicate number appears
            // update array and move slow ptr ahead
            // there are two conditions
            if (dup && nums[fast] != nums[slow]) { 
                nums[slow + 1] = nums[slow]; 
                slow += 2;
                nums[slow] = nums[fast]; 
            }
            else { slow +=1; nums[slow] = nums[fast]; }

        }
        return slow + 1;
    }
};
```


##Optimization by Using Range Loop

We can optimize the code by replace the fast pointer with range loop


```
for (auto n : nums)
```

Thus, we don't need to consider the corner cases, saving us much time.


```
int removeDuplicates(vector<int>& nums) {
    int i = 0;
    for (int n : nums)
        if (i < 2 || n > nums[i-2])
            nums[i++] = n;
    return i;
}
```


##Experience Learned

1. When traversing array and comparing elements, if comparing a[i] and a[i+j] is difficult, try doing it in the inverse way


2. range loop `for (auto var: collection)` can serve as a fast pointer in double pointer problems

