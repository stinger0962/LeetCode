# Kth Largest Element in an Array



Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

For example,
Given``` [3,2,1,5,6,4] ```and k = 2, return 5.




---



### Multiple Methods Available

There are at least 4 or 5 methods available to solve the problem. I will provide a solution using quick selection, which is an adoption of quicksort algorithm.


###Quick Selection


> For more about quicksort, see online tutorials.



Quick selection is almost same as quicksort. The only difference is that in quick selection we only check one side of a chosen pivot. 

For example, we are to find the 5th largest number, and our first pivot happens to be the 7th largest.

Since we know that the 5th largest is among the 6 larger numbers, we only need to run partition again on the 6 larger numbers. 

In a normal case, time complexity of a quick selection is T(n) = T(n/2) + n, where n is the run time of a partition. Thus, the run time is O(N) on average case, but it could increase to O(N*N) is worst case.


###The Code

```
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        int left = 0;
        int right = nums.size() - 1;
        while(true){
            int pos = partition(nums, left, right); 
            if(pos == k-1){// 0 based, kth element has index k-1
                return nums[pos];
            }
            else if(pos < k - 1){
                left = pos + 1;
            }
            else{
                right = pos - 1;
            }
        }
    }
    // Partition a sub array so that numbers left to pivot are less than 
    int partition(vector<int>& nums, int left, int right){
        int i = left; // The position to swap
        int pivot = nums[right]; // Use the rightmost element as pivot
        for(int j = left; j < right; j++){
            if(nums[j] > pivot){
                swap(nums[i], nums[j]);
                i++; // Increment i by 1 each time a number less than pivot is found
            }
        }
        swap(nums[i], nums[right]); // Put pivot into right position
        return i;
    }
};
```