# Remove Linked List Elements

Remove all elements from a linked list of integers that have value val.

Example
```
Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6
Return: 1 --> 2 --> 3 --> 4 --> 5```




---

To keep the traversal simple and to avoid treating the head differently, we can borrow the idea from *3.1 Add Two Numbers* of using **a dummy head** whose next pointing to the real head.

As we can see, **dummy head** is generally **a good idea** when dealing with a linked list, with **dummy->next** being the actual head of linked list.

Now the process is straightforward:
1. set the current to dummy head. 
2. 
** traversal** the linked list while checking curr->next->val
3. if value matches, set **curr->next to curr->next->next**
4. else **move current pointer ahead**
5. repeat 3 and 4 until we meet the end

**Notice** that when a matching value is found, we don't move the current pointer ahead. Instead, we move its next pointer ahead to take into consideration of the cases of **consecutive matching values**.


```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution 
{
public:
    ListNode* removeElements(ListNode* head, int val) 
    {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* curr = dummy;
        while(curr != nullptr)
        {
            if(curr->next != nullptr && curr->next->val == val)
            {
                curr->next = curr->next->next;
            }
            else
            {
                curr = curr->next;
            }
        }
        return dummy->next;
    }  
};```