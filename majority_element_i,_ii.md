# Majority Element I, II

Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.


Note: Alongside with the 2nd problem in its series, we want to achieve a solution using O(N) time and O(1) extra space. 
---



###Moore's Majority Vote Algorithm


  There exists a brilliant algorithm to find out the most frequently appearing element in an array.
  
  It is invented by two CS scientists in Taxes in 1980. 
  
  Let's use an example to explain its mechanism.
  
  Saying all elements in the array are party members who will vote for a ruling party. The algorithm mimics a voting system.
  
  In each turn, each voter (element in the array) will vote up if his party is in lead, or vote down if other party is in lead. 
  
  We use a variable to record the current leading party, and a counter to record the up votes. When the counter is down to zero, the leading party will change hand.
  
  By this way, after we traversal the entire array, the candidate we have is guaranteed to be the one who has most frequency.
  
  
  Pseudo code is as below
  
 ```
 Starting from cand = a[0], count = 1.
 For each a[i] in the array, starting from a[1]
   If count = 0
     cand = a[i]
     count++
   Else
     if a[i] = cand
         count++
     else
         count--
 ```
  
 