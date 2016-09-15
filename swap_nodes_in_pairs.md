# Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head.

For example,
Given ```1->2->3->4```, you should return the list as ```2->1->4->3```.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.



---


###The Algorithm

As always, we add a **dummy** head to our linked list, and **dummy->next** will be our return value.

The example in the problem doesn't show a clear connection since it lack node previous to ```1```

In the following example, it is clear that in **each swap** of two nodes, we need to **alter three connections**.

```
For each node curr, When both A and B exist, we swap A and B

curr->A->B->N   -->>>--   curr->B->A->N, 

In the process, we alter 3 connections, curr->next, A->next, and B->next.

1. Store B in a temporary pointer, 
2. Take B out of the list, 
3. Put temp pointer back to correct location.


```

###The Code

```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode dummyHead(0);
        dummyHead.next = head;
        ListNode* curr = &dummyHead; // Starting from dummy head, swap every two nodes after current node

        while(curr->next && curr->next->next){
            ListNode* temp = curr->next->next;
            curr->next->next = temp->next;
            temp->next = curr->next;
            curr->next = temp;
            curr = curr->next->next;
        }
        return dummyHead.next;
    }
};
```
