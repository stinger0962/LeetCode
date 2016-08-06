# Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.




---

### Requirement for a height balanced tree

The tree must meet two requirements to be a balaned one.

1. The difference between each node's left and right subtree must NOT be greater than 1

2. Each node's left and right subtree must be a balanced tree.  




###The Algorithm

Like other tree related algorithms, we use recursion to solve this problem. 

The algorithm is straightforward. 

We determine if a node's left/right subtree is balanced and calculate the node's height in the same function.

Thus the recursive function needs **an additional parameter to record the height (by ref).**

### The Code

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
    bool isBalanced(TreeNode* root) {
        int height = 0;
        return isBalancedHelper(root, height);
    }
    
    bool isBalancedHelper(TreeNode* root, int& height){
        if(root == nullptr){
            height = 0;
            return true;
        }
        int leftHeight = 0, rightHeight = 0;
        bool isLeftBalanced = isBalancedHelper(root->left, leftHeight);
        bool isRightBalanced = isBalancedHelper(root->right, rightHeight);
        height = max(leftHeight, rightHeight) + 1;
        if(leftHeight - rightHeight > 1 || rightHeight - leftHeight > 1){
            return false;
        }
        else{
            return isLeftBalanced && isRightBalanced;
        }
    }
};
```

