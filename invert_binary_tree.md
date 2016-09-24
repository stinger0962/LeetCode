# Invert Binary Tree

Invert a binary tree.

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9```
to
```
     4
   /   \
  7     2
 / \   / \
9   6 3   1```



---


###Recursion

Following these logics, it is easy to build a recursive function

1. What is the return type
2. What happens to the left and right child node

However, there is a place requiring extra attention in the code. 

The basic operation is wap, so we need a **temporary variable** to store one child node.

###Iteration

We use a stack/queue to do a level order traversal.

For each node, we swap its left and right child node, then push its left and right node onto the stack.



---


###The Code

*Recursion*

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
    TreeNode* invertTree(TreeNode* root) {
        return invertHelper(root);
    }
    
private:
    TreeNode* invertHelper(TreeNode* root){
        if(!root){
            return nullptr;
        }
        // There is a trap here.
        // We have to use a temp to store root->left
        // root->left is not available in the second call because it is changed in the first call
        TreeNode* temp = root->left;
        root->left = invertHelper(root->right); // first call
        root->right = invertHelper(temp); // second call
        return root;
    }
};
```

*Iteration*

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
    TreeNode* invertTree(TreeNode* root) {
        return invertHelper(root);
    }
    
private:
    TreeNode* invertHelper(TreeNode* root){
        if(!root){
            return nullptr;
        }
        queue<TreeNode*> level;
        level.push(root);
        while(!level.empty()){
            TreeNode* curr = level.front();
            level.pop();
            swap(curr->left, curr->right);
            if(curr->left) level.push(curr->left);
            if(curr->right) level.push(curr->right);
        }
        return root;
    }
};
```

