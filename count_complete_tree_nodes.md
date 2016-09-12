# Count Complete Tree Nodes


Given a complete binary tree, count the number of nodes.

Definition of a complete binary tree from Wikipedia:
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.


###Property of Complete Binary Tree

We will take advantage of the properties of complete tree to solve this problem. Otherwise, we will end up with time limit exceeding.

Let's say we have a complete binary tree of height h starting at root.

One of the two conditions must be true:

```
 A.  left subtree and right subtree have the same height, 
     There is at least one last level leaf in the right subtree.
     root->left is a perfect subtree with height h-1. 

 B.  left subtree's height = right subtree's height + 1
     All of the last level leafs are in the left subtree.
     root->right is a perfect subtree with height h-2. 
```

We know the number of nodes in a perfect tree with height h. Thus **in each step** we are able to turn the problem into a **subproblem** of finding the number of nodes in one subtree. 

We will also use the following properties to calculate the height in each step.

```
1. Both subtrees of a complete tree are themselves complete trees.

2. Height of a complete tree is the number of left most nodes in the tree.```

###Time Complexity
There are O(LogN) steps. While each step we need to find the height of the tree, which also takes O(LogN) time. Thus, the total time complexity is O(logN * logN).
