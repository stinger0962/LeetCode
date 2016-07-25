# Construct Binary Tree from Preorder and Inorder Traversal


Given preorder and inorder traversal of a tree, construct the binary tree.



---


First of all, let's have a brief discussion of **constructing a tree** in general.

The most **common procedure** involves building nodes **recursively**, from root to leaf. 

Pseudo code is as below: 

```
Node* recurBuildTree(para list)
{
  // 1. Ending conditions: usually involving reaching of boundary
  if(...)
    return nullptr;
  // 2. Construct the root node
  Node* root = new Node(val);
  // and any necessary alters on parameters which will be passed into recursive functions below
  // 3. Recursion: construct root->left, root->right by calling this function itself
  root->left = recurBuildTree(altered para list);
  root->right = recurBuildTree(altered para list);
  // 4. Return to the function caller
  return root;
}
```



---

Now that we need to build 