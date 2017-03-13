You need to construct a binary tree from a string consisting of parenthesis and integers.

The whole input represents a binary tree. It contains an integer followed by zero, one or two pairs of parenthesis. The integer represents the root's value and a pair of parenthesis contains a child binary tree with the same structure.

You always start to construct the**left**child node of the parent first if it exists.

**Example:**  


```
Input:
 "4(2(3)(1))(6(5))"

Output:
 return the tree root node representing the following tree:

       4
     /   \
    2     6
   / \   / 
  3   1 5   

```



**Note:**  


1. There will only be
   `'('`
   ,
   `')'`
   ,
   `'-'`
   and
   `'0'`
   ~
   `'9'`
   in the input string.
2. An empty tree is represented by
   `""`
   instead of
   `"()"`
   .



---



The process has two parts.

1. To retrieve a numeric value from string. The value could have multiple digits or +/- signs.

    There is a elegant and common algorithm for this part -- memorize or be familiar with it.

    2. To identify the beginning and ending of left/right sub tree, thus to build the tree by recursively call the function itself.

    Core of this part is being able to recognize pair of parentheses -- the right sub tree starts when there is no more open parentheses.



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
    TreeNode* str2tree(string s) {
        if (s.empty())
            return nullptr;
        int sz = s.size();
        int i = 0; // aux index to help extract first number
        int val = 0; // the retrieved value of root
        int sign = 1; // plus/minus sign
        // retrieve the first numeric value
        while (i < sz && ((s[i] <= '9' && s[i] >= '0') || s[i] == '-') ) {
            if (s[i] == '-')
                sign = -1;
            else
                val = val * 10 + sign * (s[i] - '0');
            ++i;
        }
        TreeNode* root = new TreeNode(val);
        // construct left subtree and right subtree
        if (i == sz) return root;
        int open_paren = 0; // record number of open parentheses
        int j = i; // aux index to locate the beginning of right subtree
        
        // loop through the string, right subtree begins when all parentheses are paired
        for (; j < sz; ++j) {
            if (s[j] == '(')
                ++open_paren;
            if (s[j] == ')')
                --open_paren;
            if (open_paren == 0)
                break;
        }
        // construct left and right subtree by calling the function itself recursively
        // skip the wrapping () of each subtree
        root->left = str2tree(s.substr(i+1, j-i-1)); // [i+1, j) index j is closing parenthesis
        if (j+2 < sz)
            root->right = str2tree(s.substr(j+2, sz-j-3)); // [j+2, sz-1) // index sz-1 is the closing parenthesis
        return root;
    }
};
```



