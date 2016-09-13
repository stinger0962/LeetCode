# Binary Tree Maximum Path Sum

Given a binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path does not need to go through the root.

For example:
Given the below binary tree,

```
       1
      / \
     2   3```
Return ```6```.




---


###The Idea

First, let's make some clarifications. A path is refered to **a collection** of nodes of which any two consecutive nodes are linked by a parent-child connection. 

This is why in the example, the answer is 6. We can go up, then go down, or vice versa. Whatever we choose our path, there must be a node that sits **in the top** of the collection.



**Our goal is thus to find such a node and its max sum, which is also the best sum.**

To find the **best sum**, we traversal the tree, calculate max sum for each node where it is the top of the path, and compare the maximum sums to get the best one.

To calculate **max sum** of a node, we need to think of its path as:  

  one **downward** path to the left, the node itself, and one **downward** path to right.
  
These subpasses have to be of single direction. Otherwise, they will not combine to a single path.

At this point, it is clear that our recursion will return the **max sum of a downward path**.

And we will need an additional variable to store the max sum of a node.

The max sum looks like:

```sum(node) = downpath(node->left) + downpath(node->right) + node->value``` 

There is still one more step left. What if a downward path is negative? 

The answer is if it is negative, we replace it with zero, which means the best sum doesn't go through that path.

```
downpath(node->left) = max(sum(node->left), 0)

downpath(node->right) = max(sum(node->right), 0)```

###Recursion Doesn't Directly Return the Solution

The hard part of this problem is that its recursive function doesn't directly return what we are looking for, but we need to find our result upon **combination of recursion of both left and right child node**.


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
    int maxPathSum(TreeNode* root) {
        int bestSum = 0;
        maxPathSumHelper(root, bestSum);
        return bestSum;
    }
    
private:
    // This function returns the maximum sum of a downward path from root
    // bestSum stores the best maximum sum we've found so far
    int maxPathSumHelper(TreeNode* root, int& bestSum){
        if(!root){
            return 0;
        }
        // Left is the maximum sum of a downward path from root->left, and
        // it is replaced by zero when negative
        int left = max(maxPathSumHelper(root->left, bestSum), 0);
        int right = max(maxPathSumHelper(root->right, bestSum), 0);
        // Update best sum when max sum of a node is greater than the current best sum
        // Max sum of a node is the max sum of left downpath + node's value + max sum of right downpath
        bestSum = max(bestSum, left + right + root->val);
        return max(left, right) + root->val;
    }
};
```