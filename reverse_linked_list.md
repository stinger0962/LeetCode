# Reverse Linked List

Reverse a singly linked list.



---


My first intuitive solution was not perfect. I used a vector to store all the nodes in order and pop out them reversely. 

A better solution involves using **two extra pointers** (besides the current pointer) to **store the previous and next node**, respectively.

The steps are really consice.

1. Use previous to store the imaginary null pointer before head
2. Set current to head and start loop
3. In the loop, we set current's next to previous, then move both previous and current forward
4. Return previous as new head (current becomes the tail null pointer)


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
    ListNode* reverseList(ListNode* head) 
    {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while(curr != nullptr)
        {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
};
```

