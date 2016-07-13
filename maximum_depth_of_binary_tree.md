# Maximum Depth of Binary Tree

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.




---

All we need is a little inspiration.

For **any node** in the tree, to **find the height of its subtree**, we will need :

**Compare the height of its left and right subtree, take the bigger one, then  +1 (the node itself).**

Then we can solve the problem by calling this process **recursively**. 

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
class Solution 
{
public:
    int maxDepth(TreeNode* root) 
    {
        if(!root)
        {
            return 0;
        }
        return helperMaxDepth(root);
    }
private:
// Concept : 
// to get the height for any node in the tree, 
// get the max of the height of its left and right subtree, then plus 1
// recursively call the function
    int helperMaxDepth(TreeNode* root)
    {
        if(!root)
        {
            return 0;
        }
        return 1 + max( helperMaxDepth(root->left), helperMaxDepth(root->right) );
    }
};

```

