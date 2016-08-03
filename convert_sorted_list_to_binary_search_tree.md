# Convert Sorted List to Binary Search Tree


Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.



---


The simplest way to solve this problem is to loop through the linked list, converting data sourse into an sorted array. The rest is exactly the same as *2.8 Convert Sorted Array to Binary Search Tree*. 
Time complexity is thus O($$N$$+$$logN$$)

Some algorithms in the discuss uses two pointers, one moving twice as fast as the other, to find the middle pointer. 
Time complexity is O($$NlogN$$). However, this approach destroies the structure of the linked list, leaving dangling nodes unreachable in the memory. By the above reason, we will not recommend using that approach. 


---


```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
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
    TreeNode* sortedListToBST(ListNode* head) 
    {
        if(head == nullptr)
        {
            return nullptr;
        }
        // Transfer the ascending linked list to a sorted array
        vector<int> data;
        ListNode* curr = head;
        while(curr != nullptr)
        {
            data.push_back(curr->val);
            curr = curr->next;
        }
        return buildBSTHelper(data, 0, data.size()-1);
    }
    
private:
    TreeNode* buildBSTHelper(vector<int>& data, int start, int end)
    {
        if(start > end)
        {
            return nullptr;
        }
        int index = (start + end + 1) / 2;
        TreeNode* root = new TreeNode(data[index]);
        root->left = buildBSTHelper(data, start, index - 1);
        root->right = buildBSTHelper(data, index + 1, end);
        return root;
    }
};
```

