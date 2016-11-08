# Path Sum III

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

Example:

```root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8```

    ```  
      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1```

Return 3. The paths that sum to 8 are:

```
1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```


###Not a Backtrack Problem

Seemingly another backtracking problem because of the keyword "path sum", this problem differs from backtracking problem in that the route does not have to be from the root. Because we do not know where to start the route, we cannot apply general backtrack algorithm on this problem.

We can tackle it down using traditional recursive algorithm of tree. 

###Mistake made

In the first attempt, I failed to recognize the two different patterns -- once we started a route, we cannot stop it. Thus we need a variable to mark it as started.

###The Code

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
    int pathSum(TreeNode* root, int sum) {
        return pathSumHelper(root, sum, true) + pathSumHelper(root, sum, false);
    }
private:
    int pathSumHelper(TreeNode* root, int sum, bool started) {
        if (!root) return 0;
        if (started) {
            return pathSumHelper(root->left, sum - root->val, true)
                + pathSumHelper(root->right, sum - root->val, true)
                + (sum == root->val ? 1 : 0);
        }
        else {
            return pathSumHelper(root->left, sum, false)
                + pathSumHelper(root->right, sum, false)
                + pathSumHelper(root->left, sum, true)
                + pathSumHelper(root->right, sum, true);
        }
    }
};
```

