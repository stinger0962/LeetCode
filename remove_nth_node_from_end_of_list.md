# Remove Nth Node From End of List

Given a linked list, remove the nth node from the end of list and return its head.

For example,

   Given linked list: ```1->2->3->4->5```, and ```n = 2```.

   After removing the second node from the end, the linked list becomes ```1->2->3->5```.
   
 
Note:
Given n will always be valid.

Try to do this in one pass.



---


###Solution without DP

It's simple and brain numbing if we use an additional data structure to store nodes while traversal to the end.

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(!head || !head->next){
            return nullptr;
        }
        ListNode* curr = head;
        vector<ListNode*> list;
        while(curr){
            list.push_back(curr);
            curr=curr->next;
        }
        int k = list.size();
        // If tail is deleted
        if(n==1){
            list[k-n-1]->next = nullptr;
        }
        // If head is deleted
        else if(n==k){
            head = head->next;
        }
        // General case
        else{
            list[k-n-1]->next = list[k-n+1];
        }
        return head;
    }
};
```


###Using Double Pointers

Moving right pointer N nodes first, then move both pointers forward until right node hits the end.

At that time, left node is assured to be the Nth node from end.



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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(!head || !head->next){
            return nullptr;
        }
        ListNode* left = head;
        ListNode* right = head;
        // Move right n nodes ahead
        // Then begin move both pointers until right becomes nullptr
        for(int i = 0; i < n; i++){
            right = right->next;
        }
        // If right is nullptr, then n equals to the length of the list
        // We are deleting head
        if(!right){
            return head->next;
        }
        // Move right to the tail of the list
        // Move left to the previous node to the one we are deleting
        while(right->next){
            left = left->next;
            right = right->next;
        }
        left->next = left->next->next;
        return head;
    }
};
```
