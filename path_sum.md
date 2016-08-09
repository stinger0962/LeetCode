# Path Sum

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

For example:
Given the below binary tree and ```sum = 22```,
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1```
return true, as there exist a root-to-leaf path ```5->4->11->2``` which sum is ```22```.




---



###Boolean Recursive Function

The common structure of a recursive function that returns boolean is as below:

```
function(parent)
  con1: return false
  con2: return true
  if (function(child) ): return true // Line 3
  return false // Line 4
```
OR
```
function(parent)
  con1: return false
  con2: return true
  if (!function(child) ): return false // Line 3
  return true // Line 4
```

Pay attention to line 3 and 4. For those **solution existence problem**, once a solution is found at a child node, all the recursive callers, up to the root should also be marked as solution found. 


###The Code

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
    bool hasPathSum(TreeNode* root, int sum) {
        return hasPathSumHelper(root, sum);
    }
    
private:
    bool hasPathSumHelper(TreeNode* root, int sum){
        if(!root){
            return false;
        }
        if(!root->left && !root->right && root->val == sum){
            return true;
        }
        if(hasPathSumHelper(root->left, sum - root->val) || hasPathSumHelper(root->right, sum - root->val) ){
            return true;
        }
        return false;
    }
};
```
