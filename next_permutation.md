# Next Permutation


Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place, do not allocate extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1```




---

###Ancient Algorithm 700 Years Old
There is an ancient algorithm to find next permutation. Pay a visit to the following [Wiki](https://en.wikipedia.org/wiki/Permutation#Generation_in_lexicographic_order) page if you are interested.



There are three steps in the algorithm which I have embedded into comments.

###The Code
```
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int len = nums.size();
        // Find the partition index
        // Starting from right, it is the first number that violates ascending order
        // e.g. in 136521 it is 3. 
        int partition = -1;
        for(int i = len - 1; i > 0; i--){
            if(nums[i] > nums[i-1]){
                partition = i-1;
                break;
            }
        }
        // Current combination is the largest possible permutation
        if(partition == -1){
            reverse(nums.begin(), nums.end());
            return;
        }
        // Starting from right, find the first number that is greater than the number 
        // at partition index and swap them
        // e.g. in 136521 it is 5. After swap, the new number becomes 156321
        int greater = -1;
        for(int i = len - 1; i > partition; i--){
            if(nums[i] > nums[partition]){
                greater = i;
                break;
            }
        }
        swap(nums[greater], nums[partition]);
        // Reverse all the numbers right to the partition index
        // Number in the above example becomes 151236, which is the next permutation of 136521
        reverse(nums.begin() + partition + 1, nums.end());
    }
};
```
