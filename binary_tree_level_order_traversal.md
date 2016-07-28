# Binary Tree Level Order Traversal

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree [3,9,20,null,null,15,7],
```    
    3
   / \
  9  20
    /  \
   15   7```
return its level order traversal as:
```
[
  [3],
  [9,20],
  [15,7]
]```



---

At the first glance, this is a **breadth-first** tree traversal. The core of a breadth-first traversal is to **use a queue**, as demonstrated below.

```
let Q be an empty queue

if the tree is not empty:
    enqueue the root of the tree into Q

while the queue is not empty:
    dequeue a node n from Q
    visit n's data
    enqueue n's children into Q```

However, the above algorithm traverses the tree in a single run. We need to make adaptions so that the algorithm **makes cuts** at the end of each level.

One such adaption is to add an inner loop, each of which represents one level in the tree.

```
if the tree is not empty:
    enqueue the root of the tree into Q

while the queue is not empty:
    record # of nodes in Q
    for # times (inner loop)
        access a node n from Q
        visit n's data
        enqueue n's children into Q
        dequeue n from Q
    deal with the collected data of that level
    ```




---

Below is the implementation. 


```/**
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
    vector<vector<int>> levelOrder(TreeNode* root) 
    {
        vector<vector<int>> valueLevelOrder; // The returning collection of nodes' values in level order
        queue<TreeNode*> nodesByLevel; // A FIFO collection storing nodes of the tree in level order. It's the key element of a breadth-first tree traversal.
        if(root == nullptr) // Treat root with care
        {
            return valueLevelOrder;
        }
        else
        {
            nodesByLevel.push(root);
        }
        while(nodesByLevel.size() != 0) // The traversal comes to an end when all the nodes in the queue are consumed.
        {
            int count = nodesByLevel.size(); // the number of nodes on the current level
            vector<int> valueByLevel; // the collection of nodes' values of the current level
            
            // Each loop represents a level, times of loop is the same as the number of the nodes in that level
            for(int i = 0; i < count; ++i) 
            {
                TreeNode* node = nodesByLevel.front();
                // For each node, do three things.
                // 1. Add its value into the vector of value. 
                // 2. Add its child nodes, if any, into the queue.
                // 3. Pop itself out of the queue.
                valueByLevel.push_back(node->val);
                if(node->left != nullptr)
                {
                    nodesByLevel.push(node->left);
                }
                if(node->right != nullptr)
                {
                    nodesByLevel.push(node->right);
                }
                nodesByLevel.pop();
            }
            valueLevelOrder.push_back(valueByLevel);
        }
        return valueLevelOrder;
    }
};```