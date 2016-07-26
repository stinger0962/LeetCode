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


 Now we come to the details of the implementation.
 
The above algorithm requires us to identify the root node first. With a careful inspection, we will find that the **1st element in preorder traversal is the root node**. 

This property also leads us to **use index in preorder to build the tree**, while using that of **inorder as a supplement**.

Next step is to **identify root->left and root->right**. To the nature of preorder traversal, **root->left is the next node to the root**, if it exists. Similarly, **the distance between root and root->right is the length of root's left subtree**. 

How do we get the length of root's subtree? **We can get the information from inorder**.  

For every node N in the inorder traversal, there exists a range of **[L... N... R]** such that **[L...N-1] is node's left subtree**, and [N+1...R] is its right subtree. 


> **Offtopic** : [L...N...R] itself is a complete tree rooted at N. This explains why inorder traversal is so important in implementing recursion.

**Now our task is to find L,N,R respectively for each node** in the tree.

We can** find N by searching the inorder** traversal for the node in preorder. ( Applying a hashtable here will reduce search time to O(1))

For **root**, L is the starting index and R  being the ending one. 

For **root->left**, L is still the starting index, but R becomes root-1.

For **root->right**, L becomes root+1, while R is still the ending one.

Let's wrap up our algorithm by choosing correct parameters in the recursive function:
* Ref to preorder and inorder traversal, ref to hashtable which stores the inorder traversal
* Index of the currently building node in preorder traversal.
* The node's left and right boundary(L & R) in inorder traversal.




---

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
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) 
    {
        // Construct a hashtable which stores info in the inorder array
        // We will use this hashtable to implement search functions
        // Key: a node's value
        // Value: a node's index in inorder vector
        unordered_map<int, int> inorderHash;
        for(int i = 0; i < inorder.size(); ++i)
        {
            inorderHash[inorder[i]] = i;
        }
        
        return buildTreeHelper(preorder, inorder, inorderHash, 0, 0, inorder.size());
    }
private:
    // The recursive helper function
    // Parameters: Besides the ref to collections, 
    // preIndex: the index in the preorder traversal of the current constructing node
    // inStart, and inEnd: the left and right boundary in the inorder traversal, all nodes between which are the current node's child nodes  
    TreeNode* buildTreeHelper(vector<int>& preorder, vector<int>& inorder, unordered_map<int, int>& inorderHash, 
                                int preIndex, int inStart, int inEnd)
    {
        // Ending conditions:
        if(preIndex == preorder.size() || inStart > inEnd)
        {
            return nullptr;
        }
        // Construct a new node for preIndex, and find its index in the inorder traversal, 
        // The search function is implemented by a hashtable, reducing time complexity to O(1)
        TreeNode* root = new TreeNode(preorder[preIndex]);
        int inIndex = inorderHash[preorder[preIndex]];
        
        // Construct root->left and root->right by calling the function itself recursively
        root->left = buildTreeHelper(preorder, inorder, inorderHash, preIndex+1, inStart, inIndex-1);
        
        root->right = buildTreeHelper(preorder, inorder, inorderHash, preIndex+inIndex-inStart+1, inIndex+1, inEnd);
        
        return root;
    }
};```


By introducing the hashtable, the runtime of this code beats 94% of others.
