# Range Sum Query I, II - Immutable

Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

```
Example:
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3```
Note:

You may assume that **the array does not change**.

There are **many calls** to sumRange function.



---


###Clarification

The key word of this problem is "immutable". The problem is based on the assumption that the array is not changed, and there will be a lot of calls. 

Thus, we need to optimize the brute force function which will take linear time each time we call the function.

###Overlapping Subsolution -- Dynamic Programming

We notice that, among many calls, some subsolutions will be calculated multiple times. For example, sum(1,4) will be calculated one more time in the call of sum(1,7). 

Therefore, dynamic programming is a good fit for such a problem with overlapping subsolutions.

Intuitively, we can use a 2D array to store all possible range sums. (e.g. sum[i][j] refers to the sum of index i to index j.)

We can further optimize the algorithm by reducing 2D array to 1D array. In the array we store the accumulated sum from beginning to that index. Range sum is thus the difference of two accumulated sums.

For example, acc[3] means accumulated sum up to index 3, and acc[6] means accumulated sum up to index 6.

Then ```range sum(4,6)``` is ```acc[6] - acc[3]```.

To achieve a clean implementation, we will add **a dummy head** in the front of the accumulated array.

###The Code

```
class NumArray {
    // An array stores all accumulated sum, (e.g.) a[i] = nums[0] + nums[1] + ... + nums[i]
    vector<int> acc;
public:
    NumArray(vector<int> &nums) {
        acc.push_back(0); // Add a dummy head into the array to achieve a clean implementation.
        for(int num: nums){
            acc.push_back(acc.back() + num);
        }
    }

    int sumRange(int i, int j) {
        return acc[j+1] - acc[i]; // Since the first element in the array is a dummy
    }
};


// Your NumArray object will be instantiated and called as such:
// NumArray numArray(nums);
// numArray.sumRange(0, 1);
// numArray.sumRange(1, 2);
```




# Range Sum Query 2D - Immutable

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner ```(row1, col1)``` and lower right corner ```(row2, col2)```.

Range Sum Query 2D
The above rectangle (with the red border) is defined by ```(row1, col1) = (2, 1)``` and ```(row2, col2) = (4, 3)```, which contains sum = 8.

```Example:
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12```

Note:
You may assume that the matrix does not change.

There are many calls to sumRegion function.

You may assume that row1 ≤ row2 and col1 ≤ col2.


###2D Version

With the inspiration from the last problem in the series, we naturally come up with using a DP table to store accumulated sum.

For example, ```acc[1][1]``` will store the sum of matrix[0][0], [0][1], [1][0], and [1][1]. 

This preprocessor has O(m*n) in time complexity, and each call to the function takes O(1). 

As with the 1st in series, we will add a dummy col and a dummy row to the top left for a clean implementation.




###The Code

