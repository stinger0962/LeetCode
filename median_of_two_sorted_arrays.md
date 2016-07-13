# Median of Two Sorted Arrays

There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

Example 1:
```
nums1 = [1, 3]
nums2 = [2]
The median is 2.0```


Example 2:
```
nums1 = [1, 2]
nums2 = [3, 4]
The median is (2 + 3)/2 = 2.5```


---

At the first glance, it is a troublesome problem. Indeed, it is lengthy and difficult if we attempt to solve it via normal approaches. We use **recursion** to achieve a concise, easy to understand solution.

First, we can generalize the particular situation "finding median" into "finding k-th smallest element of two sorted arrays". Then we can apply some restriction on k to find the median.

Assuming **k-th** is in array A, then all elements in A left to k-th (let the **number of those elements** be **a**) is less than k-th; 
likewise, in array B, let **the number of elements smaller than k-th** be **b**. 

We can make one observation, **a + b = k - 1**. 
(To be convient, we will include k-th into a so that **a + b = k**)

Therefore, our task is reduced to **find the correct value of a and b**.

To find a and b at a time requires complex algorithm. It is easy to **find a and b piece by piece using recursion.**  

1. **Each time** we make a **guess** of a and b, which satisfies **a + b = k**.
2. We can prove that [**the first a elements in A**] or [**the first b elements in B**] are ensured to be smaller than k-th. We call that [**the sorted collection**] (see details in comments of the code)
3. **Repeat the process** again, **exclude** the sorted collection, and take **new k** as k - a or k - b.
4. **The process finishes** either elements of one array is all excluded or k is reduced to 1




```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) 
    {
        int m = nums1.size();
        int n = nums2.size();
        int sum = m + n;
        if(sum%2 == 1)
        {
            return findkthElement(nums1, m, nums2, n, sum/2 + 1, 0, 0);
        }
        else
        {
            return ( findkthElement(nums1, m, nums2, n, sum/2, 0, 0) + findkthElement(nums1, m, nums2, n, sum/2 + 1, 0, 0) ) / 2;
        }
    }
    // recursive helper function
    // generalization to find k-th element in two arrays
    // each recursion attempts to make a cut, evaluates the cut, and calls itself with a smaller k
    // parameters:
    // nums1, nums2: two arrays
    // m, n: dynamic numbers of elements in the arrays(as arrays get trimmed, m,n get smaller)
    // k: we want the k-th element
    // start1, start2: the starting element to be counted (arrays will be trimmed from left during the process)
    double findkthElement(vector<int>& nums1, int m, vector<int>& nums2, int n, int k, int start1, int start2)
    {
        if(m > n)
        {
            return findkthElement(nums2, n, nums1, m, k, start2, start1);
        }
        // ending conditions:
        // either one array is entirely eliminated, or k is reduced to 1
        if(m == 0)
        {
            return nums2[start2 + k - 1];
        }
        if(k == 1)
        {
            return min(nums1[start1], nums2[start2]);
        }
        
        // recursion: 
        // cut a elements from array1 and b elements from array2
        // the sum of elements cut from two arrays should be k
        // evaluate the cut (see below)
        // call the recursion again
        int a = min(k/2, m);
        int b = k - a;
        // compare the largest numbers (to be consice, call them N1 and N2) in the cut part
        // N1 <= N2 means that N1 is smaller than at least [ (m-a)+(n-b)+1 = m+n-k+1 ] elements (*)
        // (m-a)--> the elements in arr1 right(larger) to N1
        // (n-b)+1 --> the elements in arr2 right to N2, plus N2 itself
        // the above statement(*) means N1 < k-th element and every element following k-th
        // thus N1 and every element left(smaller) to N1 are ensured to be on the left side of k-th
        // trim arr1, throw away the cut part, and call the recursion with new m=m-a, k=k-a and start1=start1+a
        if(nums1[start1 + a - 1] <= nums2[start2 + b -1])
        {
            return findkthElement(nums1, m-a, nums2, n, k-a, start1+a, start2);
        }
        else
        {
            return findkthElement(nums1, m, nums2, n-b, k-b, start1, start2+b);
        }
        
    }
};
```



