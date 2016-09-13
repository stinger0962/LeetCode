# Flatten Binary Tree to Linked List

Given a binary tree, flatten it to a linked list in-place.

For example,
Given

```
         1
        / \
       2   5
      / \   \
     3   4   6```
The flattened tree should look like:
```
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6```
click to show hints.

Hints:
If you notice carefully in the flattened tree, each node's right child points to the next node of a **pre-order traversal**.




---



### In-place operation

First of all, we want a O(1) space, O(n) run time solution. No extra data structure.

### The Algorithm

For each node, we want to cut its left, move it to right, and append right to somewhere else after left. 

``` 
     N                 N
    /  \      ->        \
   L    R                 L
  / \                      \
 ... T(tail)               ...
                             \
                              T
                               \
                                 R
```

So where is the somewhere else? We see that the flattened tree is a pre-order traversal. Thus, node->right should be appended to the right-most node of node->left subtree.

Let's have a look at both recursive and iterative solutions. 

###Recursion

Recursion is NOT easy to understand.




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
    void flatten(TreeNode* root) {
        TreeNode* flat = flattenHelper(root);
    }
    
private:
    TreeNode* flattenHelper(TreeNode* root){
        if(!root){
            return nullptr;
        }
        
        // How we determine what to return: 
        // First, the algorithm requires that left tail to be the right-most node in the flattened "subtree root->left",
        // so that we can link it with the root->right
        // ******** The key is to look at the recursive call ********
        // Look at flattenHelper(root->left), it returns the right-most node in the flattened "subtree root->left"
        // Therefore, for flattenHelper(root), it returns the right-most node in the flattened "tree root", 
        // and that is right tail, if it exists.
        // If right tail DNE, then root->right DNE. 
        // Thus root only have left subtree, the right-most of "tree root" is the right-most of "subtree root->left"
        TreeNode* leftTail = flattenHelper(root->left); 
        TreeNode* rightTail = flattenHelper(root->right);
        // If left tail exists, apply the algorithm; otherwise, left subtree DNE, no move required
        if(leftTail){
            leftTail->right = root->right;
            root->right = root->left;
            root->left = nullptr;
        }
        // Return value
        if(rightTail){
            return rightTail;
        }
        if(leftTail){
            return leftTail;
        }
        return root;
    }
};
```