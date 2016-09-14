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
        // First, let's define the left tail to be the right-most node in the flattened "subtree of root->left",
        // Left tail is to be linked with root->right 
        // The key is to look at the recursive call on root->left and on root together
        
        // Our goal is to let flattenHelper(root->left) return the right-most node in the flattened "subtree of root->left"
        // Therefore, flattenHelper(root) will return the right-most node in the flattened "tree of root" 
        // If root->right exists, returning node is also the right-most node in "subtree of root->right", 
        // which is flattenHelper(root->right)
        // If root->right DNE, returnning node is the right-most node in "subtree of root->left", 
        // which is flattenHelper(root->left)
        
        // Call recursive on both left and right child to get two useful pointers
        TreeNode* leftTail = flattenHelper(root->left); 
        TreeNode* rightTail = flattenHelper(root->right);
        
        // If left tail exists, flatten the left subtree, link left tail with root->right;
        // If left tail DNE, left subtree DNE, no movement required
        if(leftTail){
            leftTail->right = root->right;
            root->right = root->left;
            root->left = nullptr;
        }
        // RETURN
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