# Construct Binary Tree from Inorder and Postorder Traversal

Given inorder and postorder traversal of a tree, construct the binary tree.



---

**First read the following topic** : 



> *2.5 Construct Binary Tree from Inorder and Preorder Traversal*



This problem is highly similar to 2.5 except that we **traverse the postorder in the reverse order**, due to its properties. 

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
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) 
    {
        unordered_map<int, int> inorderHash; // Key: node's value, Value: node's index in inorder traversal
        for(int i = 0; i < inorder.size(); ++i)
        {
            inorderHash[inorder[i]] = i;
        }
        return buildTreeHelper(inorder, postorder, inorderHash, postorder.size()-1, 0, inorder.size()-1);
    }
private:
    TreeNode* buildTreeHelper(vector<int>& inorder, vector<int>& postorder, unordered_map<int, int>& inorderHash,
                                int postIndex, int inStart, int inEnd)
    {
        // Ending condition
        if(inStart > inEnd)
        {
            return nullptr;
        }
        // Build root node, find its index in inorder traversal
        TreeNode* root = new TreeNode(postorder[postIndex]);
        int inIndex = inorderHash[postorder[postIndex]];
        // Build root->left and root->right by calling the function recursively
        root->left = buildTreeHelper(inorder, postorder, inorderHash, postIndex-( inEnd-inIndex ) -1, inStart, inIndex-1);
        root->right = buildTreeHelper(inorder, postorder, inorderHash, postIndex-1, inIndex+1, inEnd);
        return root;
    }
};
```


### A Word about Performance



The above code has a run time of 16ms, beating 90% of others.

I actually made one **small improvement in the "ending condition"** of recursion. 

**I omitted checking the range of *postIndex***. The reason behind is that whenever we pass a new *postIndex* into next call, we also pass a unique pair of *inStart and inEnd*. Thus we can rely on the pair to check if we have reached the leaf node.

