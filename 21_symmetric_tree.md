# 2.1 Symmetric Tree

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

``` 
    1
   / \
  2   2
 / \ / \
3  4 4  3```
But the following [1,2,2,null,3,null,3] is not:
```    
    1
   / \
  2   2
   \   \
   3    3```
   
   Note:
Bonus points if you could solve it both recursively and iteratively.



   The essence of this problem lies in understanding the traversal of a binary tree. 
  
  Recursion is the easiest way to solve a tree traversal. **The common mechanism of recursion** is to use a public function/interface which calls upon a private recursive function.
  
  Notice that in a recursive function, **ending case**, or any specific case that will terminate the recursion, always comes first. Recursive call usually stands at the end of the function.
  
  ```/**
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
    bool isSymmetric(TreeNode* root) 
    {
        if(root == nullptr)
        {
            return true;
        }
        else
        {
            return isMirroring(root->left, root->right);
        }
        
    }
private:
    bool isMirroring(TreeNode* left, TreeNode* right)
    {
        if(left == nullptr && right == nullptr)
        {
            return true;
        }
        else if(left == nullptr || right == nullptr)
        {
            return false;
        }
        else if(left->val != right->val)
        {
            return false;
        }
        else
        {
            return isMirroring(left->left, right->right) && isMirroring(left->right, right->left);
        }
            
    }
};```