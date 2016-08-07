# Minimum Depth of Binary Tree


Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.




---


###Compare to Finding the Max Depth of Binary Tree

These two problems share almost same algorithm. 

To find max depth is straightforward: 

```maxHeight(root) = max( maxHeight(root->left), maxHeight(root->right) )+1```

To find min depth, we want to use:

```minHeight(root) = min( minHeight(root->left), minHeight(root->right) )+1```

However, we need to pay attention to those **nodes that only have one child node**.

For example, in this simple tree, using the above ```minHeight()```, the height of node A is 2. If we use the above ```minHeight()```, we will get the wrong answer of 1.

```
    A
  /   
 B
```
It is easy to fix the problem.

The height of a node that only has one child is the [**height of its only child + 1**].

Writing elegantly: 

```minHeight(root) = max( minHeight(root->left), minHeight(root->right) )+1```


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
    int minDepth(TreeNode* root) {
        return minDepthHelper(root);
    }
    
private:
    int minDepthHelper(TreeNode* root){
        if(root == nullptr){
            return 0;
        }
        if(root->left == nullptr || root->right == nullptr){
            return max(minDepthHelper(root->left), minDepthHelper(root->right)) + 1;
        }
        else{
            return min(minDepthHelper(root->left), minDepthHelper(root->right)) + 1;
        }
        
    }
};
```