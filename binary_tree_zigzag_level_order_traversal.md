# Binary Tree Zigzag Level Order Traversal

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
```Given 
binary tree [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7```
return its zigzag level order traversal as:
```[
  [3],
  [20,9],
  [15,7]
]```




---

Similar to *2.2 Binary Tree Level Order Traversal* , this problem requires a **breadth-first tree traversal**. The core of a BFT is still** using a queue**. However, we need an iregular, or an adopted queue to implement zigzag traversal.

After running a scenario on the paper, I figured the problem requires us to retrive/add data at both ends of the queue. There is a STL container best fit the requirement -- **deque**.

The paper simulation also uncovers that there are **two patterns** of inserting/retriving , **triggering on and off between adjacent levels**.

The task remained is thus no more than doing some changes on the implementaion of 2.2.

Runtime is 4ms, the best we can get. Notice a block of comments about **optimizing the code**.



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
class Solution 
{
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) 
    {
        vector<vector<int>> treeValues;
        if(root == nullptr)
        {
            return treeValues;
        }
        bool trigger = true;
        deque<TreeNode*> levelNodes;
        levelNodes.push_back(root);

        while(!levelNodes.empty())
        {
            int n = levelNodes.size();
            vector<int> levelValues;
            for(int i = 0; i < n; ++i)
            {
                if(trigger)
                {
                    TreeNode* node = levelNodes.back();
                    levelNodes.pop_back();
                    if(node->left != nullptr)
                    {
                        levelNodes.push_front(node->left);
                    }
                    if(node->right != nullptr)
                    {
                        levelNodes.push_front(node->right);
                    }
                    levelValues.push_back(node->val); // **Note**: could also be implemented by adding child nodes, but that will potentially double the interactions, causing inefficiency.
                }
                else
                {
                    TreeNode* node = levelNodes.front();
                    levelNodes.pop_front();
                    if(node->right != nullptr)
                    {
                        levelNodes.push_back(node->right);
                    }
                    if(node->left != nullptr)
                    {
                        levelNodes.push_back(node->left);
                    }
                    levelValues.push_back(node->val);
                }
            }

            treeValues.push_back(levelValues);
            trigger = trigger? false : true;
        }
        return treeValues;
    }
};
```