# Populating Next Right Pointers in Each Node


Given a binary tree

```
struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }```
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:

You may only use constant extra space.
You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).
For example,
Given the following perfect binary tree,
```
        1
       /  \
      2    3
     / \  / \
    4  5  6  7```
After calling your function, the tree should look like:
```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL```
    
    
    

---



### Using No Extra Data Structure

"You may only use constant extra space" means that we cannot introduce queue to do level traversal.


### Structure of Perfect Tree

If we draw a sample tree on the paper, we will find that each ```p->right->next = p->next->left```, whenever it ```p->next``` exists.

This observation makes almost 90% of the solution.

###The Code

```
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
 // Cannot use queue to store level nodes due to constraint of constant extre space 
 // The idea is, traversal and link each level starting from its leftmost node
 // Envolving using the property of a perfect binary tree
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if(!root){
            return;
        }
        TreeLinkNode* pre = root; // Leftmost pointer from previous level
        TreeLinkNode* cur = nullptr; // Pointer used to traversal current level
        while(pre->left){
            cur = pre; 
            while(cur){ // Traversal the level where the leftmost node is pre->left
                cur->left->next = cur->right;
                if(cur->next){
                    cur->right->next = cur->next->left;
                }
                cur = cur->next;
            }
            pre = pre->left; // Go to next level
        }
    }
    

};
```