# Convert Sorted Array to Binary Search Tree

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.


---


Using a **binary recursion** to build the tree is the most efficient way.


> See 2.5 Construct Binary Tree from inorder and preorder for more hint.







---


```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) 
    {
        return buildBSTHelper(nums, 0, nums.size()-1);
    }
    
private:
    TreeNode* buildBSTHelper(vector<int>& nums, int start, int end)
    {
        if(start > end)
        {
            return nullptr;
        }
        int index = (start + end) / 2;
        TreeNode* root = new TreeNode(nums[index]);
        root->left = buildBSTHelper(nums, start, index - 1);
        root->right = buildBSTHelper(nums, index + 1, end);
        return root;
    }
};
```