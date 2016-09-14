# Merge K Sorted Lists
Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.



---


###Merge Sort

This problem has a similar structure as a merge sort in that they both merge K elements into one.

We can apply **divide and conquer** algorithm to solve the problem.

The key is to write a function to merge two lists. 

As long as we have our function ```mergeTwo(list1, list2)```, we can write merge K lists into :

```mergeK(k lists) = mergeTwo(mergeK(first half lists), mergeK(second half lists))```

For more details about merging two sorted lists, please refer to *3.4 Merge Two Sorted Lists* 


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
    // Time complexity is N*Log(K)

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.size() == 0){
            return nullptr;
        }
        if(lists.size() == 1){
            return lists.back();
        }
        if(lists.size() == 2){
            return mergeTwoLists(lists[0], lists[1]);
        }
        const size_t size = lists.size();
        // Use vector's range constructor to split lists into two vectors
        vector<ListNode*> first(lists.begin(), lists.begin() + size/2);
        vector<ListNode*> second(lists.begin() + size/2, lists.end());
        // Divide and Conquer
        return mergeTwoLists(mergeKLists(first), mergeKLists(second));
    }
private:
    // Merge two sorted list, iterative solution
    // Only use O(1) extra space
    // Time complexity is N, where N is the longest length of a list
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2){
        if(l1 == nullptr){
            return l2;
        }
        if(l2 == nullptr){
            return l1;
        }
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
        mergeHead->next = l1 ? l1 : l2;
        return dummyHead.next;
    }
};
```

