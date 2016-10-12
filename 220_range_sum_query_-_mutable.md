# 2.20 Range Sum Query - Mutable

Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.
Example:
```
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```
Note:
The array is only modifiable by the update function.
You may assume the number of calls to update and sumRange function is distributed evenly.



---


###Segment Tree

To retrieve  O(logN) on both update and getSum, we need to introduce a new data structure, called **segment tree**.

Segment tree has following features.

1. It is a full binary tree to store intervals.

2. Each node is an element in an the input array.

3. Value of each internal node is the sum of its left and right child node.

4. From above features, it takes O(LogN) to retrieve range sum and update specific element in the array.

###Implementation

Each node has a pair of int representing the range of its interval.

As always, each node also has two pointers pointing to left and right child nodes, and a key value.

Build tree, update tree, retrieve range sum all share the similar structure.

In the function parameter, we use a pair of int to specify its interval```(start, end)```

Then in the function we recursively build its left and right based on ```(start, mid)``` and ```(mid, end)```, where ```mid = (start+end)/2```



---

###The Code

```
struct SegTreeNode {
    int start, end; // inclusive [start, end]
    SegTreeNode *left, *right;
    int sum;
public:
    SegTreeNode(int s, int e)
    : start(s), end(e), left(nullptr), right(nullptr), sum(0) {}
};
class NumArray {
    SegTreeNode* root;

    // Build the segment tree for range of [start, end]
    // Each recursion creates a new node, recursively calls build on left/right, then updates value of the new node
    SegTreeNode* buildTree(vector<int>& nums, int start, int end) {
        if(start > end) return nullptr;
        SegTreeNode* root = new SegTreeNode(start, end);
        if(start == end) root->sum = nums[start]; // Leaf node
        else {
            int mid = (start + end) / 2;
            root->left = buildTree(nums, start, mid);
            root->right = buildTree(nums, mid+1, end);
            root->sum = root->left->sum + root->right->sum;
        }
        return root;
    }
    
    // Retrieve element sum of range [start, end)
    int sumRange(SegTreeNode* root, int start, int end) {
        if(root->start == start && root->end == end) return root->sum; // Leaf node
        int mid = (root->start + root->end) / 2;
        if(start > mid)
            return sumRange(root->right, start, end);
        else if(end <= mid)
            return sumRange(root->left, start, end);
        else {
            return sumRange(root->left, start, mid) + sumRange(root->right, mid+1, end);
        }
    }
    
    // Update an element at index of i
    void update(SegTreeNode* root, int i, int val) {
     if(root->start == root->end) root->sum = val; // Leaf node
     else {
         int mid = (root->start + root->end) / 2;
         if(i <= mid) {
             update(root->left, i, val);
         }
         else {
             update(root->right, i, val);
         }
         root->sum = root->left->sum + root->right->sum;
     }
    }
public:
    NumArray(vector<int> &nums) {
        root = buildTree(nums, 0, nums.size()-1);
    }

    void update(int i, int val) {
        update(root, i, val);
    }

    int sumRange(int i, int j) {
        return sumRange(root, i, j);
    }
};


// Your NumArray object will be instantiated and called as such:
// NumArray numArray(nums);
// numArray.sumRange(0, 1);
// numArray.update(1, 10);
// numArray.sumRange(1, 2);
```




