# Merge Two Sorted Lists

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.



---


###A Solution Using Recursion

Natually, in each recursive call, we build one node into the merged list, remove one from two lists.

Break down into steps, in each recursion, we will do:

1. Determine *head of the merged list*. It is the smaller head of the two lists.

2. Assign next node of *head of the merged list* to the returning value of next recursive call.

3. Return *head of the merged list*. 

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1 == nullptr){
            return l2;
        }
        if(l2 == nullptr){
            return l1;
        }
        ListNode* mergeHead = nullptr;
        if(l1->val < l2->val){
            mergeHead = l1;
            mergeHead->next = mergeTwoLists(l1->next, l2);
        }
        else{
            mergeHead = l2;
            mergeHead->next = mergeTwoLists(l1, l2->next);
        }
        return mergeHead;
    }
};
```

###Alternative Solution Using Iteration

Recursion may cause stack overflow, thus we also want to try an iterative version which doesn't pose such a problem.


As always, using a dummy head will do us a lot of convenience.
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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1 == nullptr){
            return l2;
        }
        if(l2 == nullptr){
            return l1;
        }
        // Since mergeHead will move forward, we need a dummy head to store and return the head of merged list.
        // And we use a local variable instead a new pointer so that it's destroied when the function
        ListNode dummyHead(0);
        ListNode* mergeHead = &dummyHead;
        while(l1 && l2){
            if(l1->val < l2->val){
                mergeHead->next = l1;
                l1 = l1->next;
            }
            else{
                mergeHead->next = l2;
                l2 = l2->next;
            }
            mergeHead = mergeHead->next;
        }
        // At this point either l1 or l2 is nullptr, so we only need to link one more time
        mergeHead->next = l1 ? l1 : l2;
        return dummyHead.next;
    }
};
```