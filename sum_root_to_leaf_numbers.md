# Sum Root to Leaf Numbers

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,

```
    1
   / \
  2   3
  ```
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.

Return the ```sum = 12 + 13 = 25```.

Note: there is no need to eliminate duplicates.



---


###Typical Backtracking

This problem is a typical backtracking one since it asks for **all root to leaf solution**.

The common structure of backtracking for a root to leaf problem is as below:

```
DFS(Node* curr, paralist)
  // Ending condition 1
  if curr is nullptr
    return;
  // Ending condition 2
  if curr is leaf
    if path is a solution
      push path to solution set
    return;
  // Backtracking
  add candidate to path
  DFS(Node* curr->left, new paralist)
  DFS(Node* curr->right, new paralist)
  remove candidate from path
  return;
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
    int sumNumbers(TreeNode* root) {
        if(!root) return 0;
        int sum = 0;
        int path = 0;
        sumNumbersDFS(root, sum, path);
        return sum;
    }
    
private:
    // curr: current node in the tree
    // result: the sum of all root-to-leaf numbers
    // path: number constructed by current path
    void sumNumbersDFS(TreeNode* curr, int& result, int& path){
        // Ending condition 1: null pointer
        if(!curr) return;
        // Ending condition 2: leaf 
        if(!curr->left && !curr->right){
            path = path * 10 + curr->val;
            result += path; // Add a root-to-leaf number to the result
            path = (path - curr->val) / 10;
            return;
        }
        int temp = path;
        path = path * 10 + curr->val;
        sumNumbersDFS(curr->left, result, path);
        sumNumbersDFS(curr->right, result, path);
        path = temp;
        return;
    }
};
```

*Alternatively, we can solve the problem by using a pure recursive function which return sum itself.*

Logic is as below:

1. Each non-leaf node is counted once for left child, and once for right child
2. Each leaf node is counted only once
3. Return value: the sum of all root-to-leaf numbers

Code is short, but personally I prefer use backtracking as it is more systematical and practical.

To me, the second solution looks more like an invention by genius. I can enjoy its beauty, but can't  expect myself do the same in a new situation:X

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
    int sumNumbers(TreeNode* root) {
        return sumHelper(root, 0);
    }
private:
    // curr: current node in the tree
    // sum: number constructed by current path
    int sumHelper(TreeNode* curr, int sum){
        // Ending condition 1: null pointer
        if(!curr) return 0;
        // Ending condition 2: leaf 
        if(!curr->left && !curr->right){
            return sum*10 + curr->val;
        }
        return sumHelper(curr->left, sum*10 + curr->val) + sumHelper(curr->right, sum*10 + curr->val);
    }
};
```