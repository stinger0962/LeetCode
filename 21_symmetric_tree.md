# 2.1 Symmetric Tree

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

``` 
    1
   / \
  2   2
 / \ / \
3  4 4  3```
But the following [1,2,2,null,3,null,3] is not:
```    
    1
   / \
  2   2
   \   \
   3    3```
   
   Note:
Bonus points if you could solve it both **recursively** and **iteratively**.



   The essence of this problem lies in understanding the traversal of a binary tree. 
  
  Recursion is the easiest way to solve a tree traversal. **The common mechanism of recursion** is to use a public function/interface which calls upon a private recursive function.
  
  Notice that in a recursive function, **ending case**, or any specific case that will terminate the recursion, always comes first. Recursive call usually stands at the end of the function.
  
  ```/**
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
    bool isSymmetric(TreeNode* root) 
    {
        if(root == nullptr)
        {
            return true;
        }
        else
        {
            return isMirroring(root->left, root->right);
        }
        
    }
private:
    bool isMirroring(TreeNode* left, TreeNode* right)
    {
        if(left == nullptr && right == nullptr)
        {
            return true;
        }
        else if(left == nullptr || right == nullptr)
        {
            return false;
        }
        else if(left->val != right->val)
        {
            return false;
        }
        else
        {
            return isMirroring(left->left, right->right) && isMirroring(left->right, right->left);
        }
            
    }
};```


  Solving the problem iteratively boils down to the algorithm of an inorder tree traversal without recursion.
  
  Using a custom **stack** (std::vector in C++) is the way to achieve the goal. We need to store the subtree in a container with LIFO(last-in-first-out) property.
  
  Below is a common algorithm found online.
  

>   1.  Create an empty stack S.
  2.  Initialize current node as root
  3. Push the current node to S and set current = current->left until current is NULL
  4. If current is NULL and stack is not empty then 
     a) Pop the top item from stack.
     b) Print the popped item, set current = popped_item->right 
     c) Go to step 3.
  5. If current is NULL and stack is empty then we are done.


  
  My implementation traverses the left and right subtrees simultaneously, check their symmetricity in both data and shape, and terminate the loop early whenever a symmetric test fails.
  
  ```/**
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
    bool isSymmetric(TreeNode* root) 
    {
        if(root == nullptr)
        {
            return true;
        }
        vector<TreeNode*> leftTree; // using stack to traverse tree without recursion
        vector<TreeNode*> rightTree;
        TreeNode* leftCur = root;
        TreeNode* rightCur = root;
        while(leftTree.size() != 0 || leftCur != nullptr)
        {
            while(leftCur != nullptr) // simultaneously traverse left subtree inorder and right subtree reversely, terminate the loop early if certain conditions are met
            {
                if(rightCur == nullptr) // when the left subtree is deeper than the right one
                {
                    return false;
                }
                if(leftCur->val != rightCur->val) // compare value of nodes at symmetric positions
                {
                    return false;
                }
                leftTree.push_back(leftCur);
                leftCur = leftCur->left;
                rightTree.push_back(rightCur);
                rightCur = rightCur->right;
            }
            if(rightCur != nullptr) // when the right subtree is deeper than the left one
            {
                return false;
            }
            leftCur = leftTree.back()->right;
            leftTree.pop_back();
            rightCur = rightTree.back()->left;
            rightTree.pop_back();
            
        } // done with the travesal of the left subtree
        if(rightTree.size() !=0 || rightCur != nullptr) // 
        {
            return false;
        }
        return true;
    }
private:
   
};```


  There is no performance issue in this problem.