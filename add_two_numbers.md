# Add Two Numbers



You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

**Input**: (2 -> 4 -> 3) + (5 -> 6 -> 4)

**Output**: 7 -> 0 -> 8



---

  This article displays two different solutions -- one face-in-face solution which heavily depends on algorithm, and an alternative, back-stab solution.

  Let's first take a look into the alternative solution. Instead of thinking of proper algorithm that works between two linked list, we can think of this problem as two parts: 
  
  1. Transform an linked list to an integer
  2. Transform an integer to an linked list

  We can build a helper function to achieve each part. 
  
  Time complexity is O(n)+O(m)+O(the bigger one of m and n).

Assuming n > m, then the total time complexity is 3*O(n), eventually O(n). (As we will see, the performance is as same as the algorithm-ish approach)

```/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) 
    {
        unsigned long number1 = listToInt(l1);
        unsigned long number2 = listToInt(l2);
        return intToList(number1 + number2);
    }
private:
    ListNode* intToList(unsigned long number)
    {
        ListNode* head = new ListNode(number % 10);
        ListNode* current = head;
        number = number / 10;
        while(number != 0)
        {
            current->next = new ListNode(number % 10);
            current = current->next;
            number = number / 10;
        }
        return head;
    }
    unsigned long listToInt(ListNode* list)
    {
        unsigned long number = list->val;
        list = list->next;
        unsigned int exp = 0;
        while(list != nullptr)
        {
            ++exp;
            number = number + list->val * pow(10, exp);
            list = list->next;
        }
        return number;
    }
};```



---

Now let's have a look at the algorithm-ish approach.

  The process is listed as below:

  1. fetch one digit from each list
  2. calculate their sum and deal with the condition when the sum is greater or equal to 10.
  3. build a node for the result list with the value of "tweaked" sum.
  4. move forward and repeat steps 1 ~ 3. Take additional care when one list ends earlier than the other.


This implementation involves the idea of using **a dummy head in the linked list**, an inspiration for future design.


```/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) 
    {
        ListNode* ptr1 = l1;// current pointer in list 1
        ListNode* ptr2 = l2;
        int dgt1 = 0; // current digit in list 1
        int dgt2 = 0;
        int sum = 0; // sum of current digits
        int carry = 0; // if sum >= 10, carry 1 to next digit
        ListNode* dummyHead = new ListNode(0); // use dummy head can rid troublesome setting up before entering while loop
        ListNode* current = dummyHead;
        
        while(ptr1 != nullptr || ptr2 != nullptr) // terminating condition: both lists reach their ends
        {
            // take additional care when fetching digits since two lists may have different length
            dgt1 = (ptr1 != nullptr) ? ptr1->val : 0;
            if(ptr1 != nullptr)
            {
                ptr1 = ptr1->next;
            }
            dgt2 = (ptr2 != nullptr) ? ptr2->val : 0;
            if(ptr2 != nullptr)
            {
                ptr2 = ptr2->next;
            }
            
            sum = dgt1 + dgt2 + carry;
            current->next = new ListNode(sum % 10);
            current = current->next;
            carry = sum / 10; // since sum ranges from [0,19], carry is either 0 or 1
        }
        if(carry == 1)
        {
            current->next = new ListNode(1);
        }
        
        
        
        return dummyHead->next;
    }
};```




