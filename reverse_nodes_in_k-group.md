# Reverse Nodes in k-Group

Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

You may not alter the values in the nodes, only nodes itself may be changed.

Only constant memory is allowed.

For example,
Given this linked list: ```1->2->3->4->5```

For k = 2, you should return: ```2->1->4->3->5```

For k = 3, you should return: ```3->2->1->4->5```

To **clarify**, reverse every K consecutive nodes of the linked list, while the last M nodes, where M < k, are not changed.

---

###Approach by Recursion

The idea is that in each recursion, we reverse the first k nodes (if possible) and call recursion on the (k+1)th node. 

The algorithm is as below.

```
We have these steps to do in each recursion.

1. Set current pointer as head, and move forward current k times, or until the end of the list.

2. If current is not at the end of the list, call the recursion on current.

3. Reverse the first k nodes in the list.

4. Return new head.
```

We set up one node in reversed order **in each swap**.

(e.g.) k = 4, curr is the head of next group of k nodes

```
       1->2->3->4->curr  
        ---->  2->3->4->curr, 1->curr  
        ---->  3->4->curr, 2->1->curr
        ---->  4->curr, 3->2->1->curr
        ---->  curr, 4->3->2->1->curr ```



###Approach by Iteration

The approach of iteration will first count the number of nodes in the list, then reverse k nodes in a loop, repeat this process until there are not enough nodes left.

**In each swap**, we move one node(```start->next```) to the beginning of its group. (Note the difference to that of recursion) 

(e.g.) 

```
    start->2->3->4  
    ---->  2->start->3->4  
    ---->  3->2->start->4  
    ---->  4->3->2->start
```

###The Code


*Recursion*

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
    // In each recursion, we reverse the first k nodes (if possible)
    // Then call recursion on k+1 th node
    ListNode* reverseKGroup(ListNode* head, int k) {
        int count = 0;
        ListNode* curr = head;
        // Step 1: move forward current pointer to k+1 th node
        while(curr && count != k){
            curr = curr->next;
            count++;
        }
        // Reversion only happens when there are enough nodes left
        // If there are not enough nodes, nothing changes
        if(count == k){
            // Step 2: Call recursion on k+1 th node, which reverses linked list with k+1 th node as head
            curr = reverseKGroup(curr, k);
            // Step 3: Reverse the first k nodes
            while(count > 0){
                // Best way to figure out the following logic is to draw some picture and practice steps by steps
                ListNode* temp = head->next;
                head->next = curr;
                curr = head;
                head = temp;
                count--;
            }
            head = curr; // Don't forget this last step
        }
        return head;
    }
};
```

*Iteration*

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
    ListNode *reverseKGroup(ListNode *head, int k){
        if(!head || k == 1) return head;
        int count = 0; // # of nodes in the list
        ListNode dummyHead(0);
        dummyHead.next = head; // Return value
    
        ListNode* curr = head;
        while(curr){
            curr = curr->next;
            count++;
        }
        ListNode* pre = &dummyHead;
        ListNode* start = pre->next;
        ListNode* end = nullptr;
        while(count >= k){ // When there are enough nodes left, reverse nodes by group of k
            for(int i = 1; i < k; i++){ // B/c of the mechanism, we sawp k-1 times for a group
                end = start->next;
                start->next = end->next;
                end->next = pre->next;
                pre->next = end;
            }
            pre = start;
            start = pre->next;
            count -= k;
        }
        return dummyHead.next;
    }
};
```