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


