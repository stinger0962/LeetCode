# Path Sum II


Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

For example:
Given the below binary tree and sum = 22,
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1```
return
```
[
   [5,4,11,2],
   [5,8,4,5]
]```




---

###Backtracking -- Find'em All

Different from its preceding version, *2.14 Path Sum*, which is a simple tree traversal, this problem wants us to **find ALL solutions**.

**Backtracking** is a well designed algorithm to solve such a problem.

**Refer to the summary of this chapter** for more details and pseduocode of backtracking. 

###The Pseudocode

```
findPath(root, sum, path, path collection)
  if (!root) then return
  add root into solution
  if solution found, add path into path collection
  findPath(root->left)
  findPath(root->right)
  remove root from solution 
```

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
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<int> path;
        vector<vector<int>> paths;
        if(!root){
            return paths;
        }
        pathSumHelper(root, sum, path, paths);
        return paths;
    }
private:
    void pathSumHelper(TreeNode* root, int sum, vector<int>& path, vector<vector<int>>& paths){
        if(!root){
            return;
        }
        path.push_back(root->val); // Each node be a candidate of the solution
        if(!root->left && !root->right && root->val == sum){ // If a path is found, record the path
                paths.push_back(path);
        }

        pathSumHelper(root->left, sum - root->val, path, paths);
        pathSumHelper(root->right, sum - root->val, path, paths);
        path.pop_back(); // Remove node from candidate
        return;
    }

};
```