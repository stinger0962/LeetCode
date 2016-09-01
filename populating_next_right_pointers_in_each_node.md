# Populating Next Right Pointers in Each Node I, II

###Next Right Pointer I 
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
You may **assume that it is a perfect binary tree **(ie, all leaves are at the same level, and every parent has two children).
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



### Next Right Pointer II

What if the given tree could be **any binary tree**? 



---

###Breadth First Traversal

The idea is to implement a BFT without using extra data structure, such as queue.

Recall that the function of a queue in BFT is to gather all nodes at the same level.

Here we already have a next pointer to link nodes in the same level together. Taking advantage of next pointer, we can implement BFT without queue.

###A Clumsy Implementation Only Traversaling One Level a Time

In the intuitive solution, we use a pointer to traversal the tree by level, and an additional pointer pointing to the head of each next level.

As you can see, the implementation is long and hard to come up with.

```
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root) {
        TreeLinkNode* curr = root; // The current node in the traversal. Linking happens on the next level
        TreeLinkNode* tail = nullptr; // The node in next level we are linking. It's either curr->left or curr->right
        // Head of the next level. 
        // It is set to nullptr at the end of each level
        // Then reset to the first node (from left) on the next level
        TreeLinkNode* head = nullptr; 
        while(curr != nullptr){
            // Due to logical requirement, we start from checking right child, while not left
            if(curr->right){
                // If curr has both left and right child nodes, we have two additional steps to do
                // 1: link curr's left to its right
                // 2: set head to curr's left if head is nullptr
                if(curr->left){
                    curr->left->next = curr->right;
                    if(!head){
                    head = curr->left;
                    }
                }
                // Now link curr->right to its next node
                // Store curr->right as tail since curr will move forward in the process
                tail = curr->right;
                if(!head){
                head = tail;
                }
                // Move curr ahead until either reaching the end of the level or curr has at least a child node
                while(curr->next && !curr->next->left && !curr->next->right){
                curr=curr->next;
                }
                // Condition 1: End of the level
                if(!curr->next){
                    tail->next = nullptr;
                }
                // Condition 2: Has left child
                else if(curr->next->left){
                    tail->next = curr->next->left;
                }
                // Condition 3: Has right child
                else if(curr->next->right){
                    tail->next = curr->next->right;
                }   
            }
            // If the current node only has left child
            else if(curr->left){
                tail = curr->left;
                if(!head){
                    head = tail;
                }
                while(curr->next && !curr->next->left && !curr->next->right){
                    curr=curr->next;
                }
                if(!curr->next){
                    tail->next = nullptr;
                }
                else if(curr->next->left){
                    tail->next = curr->next->left;
                }
                else if(curr->next->right){
                    tail->next = curr->next->right;
                }
            }
            // We have checked and linked curr's child nodes. 
            // Move curr forward
            // If reaching the end, move it to the beginnig of next level
            if(curr->next){
                curr = curr->next;
            }
            else{
                curr = head;
                head = nullptr;
            }
        }
    }
};
```

###An Elegant Implementation Traversaling Two Levels Simultaneously

The key of an elegant solution is to use **another pointer on the next level**, and start traversaling both current and next level at the same time.


In the below implementation, ```curr``` is the pointer of current level, while ```tail``` is the pointer on the next level.

```
/**
 * Definition for binary tree with next pointer.
 * struct TreeLinkNode {
 *  int val;
 *  TreeLinkNode *left, *right, *next;
 *  TreeLinkNode(int x) : val(x), left(NULL), right(NULL), next(NULL) {}
 * };
 */
class Solution {
public:
    void connect(TreeLinkNode *root) {
        TreeLinkNode* curr = root; // The current node in the traversal. Linking happens on the next level
        TreeLinkNode* tail = nullptr; // The node being linked on the next level
        // Head of the next level. 
        // It is set to nullptr at the end of each level
        // Then reset to the first node (from left) on the next level
        TreeLinkNode* head = nullptr; 
        
        
        while(curr){
            // At the entrance of each level, current points to the head, while tail points to nullptr
            // We start checking current's L/R, and link them if found
            if(curr->left){
                // If we are at the head, set head and tail
                if(!tail){
                    head = curr->left;
                    tail = head;
                }
                // Otherwise, link nodes and move ahead
                else{
                    tail->next = curr->left;
                    tail = tail->next;
                }
                
            }
            if(curr->right){
                if(!tail){
                    head = curr->right;
                    tail = head;
                }
                else{
                    tail->next = curr->right;
                    tail = tail->next;
                }
                
            }
            // We have finished checking current's L/R, so we move current ahead
            // If we reach the end of a level, move current to the head of next level
            if(curr->next){
                curr = curr->next;
            }
            else{
                curr = head;
                head = nullptr;
                tail = nullptr;
            }
        }
    }
};
```
