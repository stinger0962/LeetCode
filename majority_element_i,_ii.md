# Majority Element I, II

Given an array of size n, find the majority element. The majority element is the element that appears more than``` ⌊ n/2 ⌋``` times.

You may assume that the array is non-empty and the majority element always exist in the array.


Note: Alongside with the 2nd problem in its series, we want to achieve a solution using O(N) time and O(1) extra space. 
---



###Moore's Majority Vote Algorithm


   There exists a brilliant algorithm to find the majority element(the one occurs more than ```n/2``` times) in an array.
  
  Two Texas CS scientists invented it in 1980. 
  
  Let's use an example to explain its mechanism.
  
  Saying that all elements in the array are members from different parties who are eligible to vote for an election. The algorithm thus mimics a voting system.
  
  Each voter (element in the array) will vote up if his party is in lead, or vote down if other party is in lead. 
  
  We record current leading party as **candidate majority**, and record up votes using a **counter**. When the counter is down to zero, the leading party will change hand.
  
  After we sweep the array, we will have a candidate majority. At this point, we are NOT 
  
  
  Pseudo code is as below
  
 ```
 Starting from cand = a[0], count = 1.
 For each a[i] in the array, starting from a[1]
   If count = 0
     cand = a[i]
     count = 1
   Else
     if a[i] = cand
         count++
     else
         count--
 ```
  
 ###The Code
 
 ```
 class Solution {
public:
    // Moore's Majority Vote Algorithm, return the majority element in the array
    // Sweep from left to right, majority number(current) will vote up and other numbers will vote down
    // When counter is down to zero, a new number takes control of MN
    // counter doesn't have any meaning after the loop is done
    int majorityElement(vector<int>& nums) {
        int major = nums[0];
        int count = 1;
        for(int i = 1; i < nums.size(); i++){
            if(count == 0){
                major = nums[i];
                count = 1;
            }
            else if(major == nums[i]){
                count++;
            }
            else{
                count--;
            }
        }
        return major;
    }
};
 ```
 
 
 #Majority Element II
 
 Given an integer array of size n, find all elements that appear more than ```⌊ n/3 ⌋``` times. The algorithm should run in linear time and in O(1) space.
 
 Hint: how many such elements could it possibly have?
 
 There are** at most 2 **such numbers.

---

We apply some changes on the Boye Moore's Voting Algorithm to generalize it: 

1. We use two candidates, 
2. We use two counters, 
3. When vote down, we vote down both counters.


More explanation are available in the comments.

###The Code

```
class Solution {
public:
    // We will modify on Boye Moore's Vote Algorithm
    // Since there are at most two such numbers, we tie them together as a "combined party", 
    // Each party member votes up for its party(+1), and each "minority" party member vote down both party(-2)
    // The point is that vote down is counted on both party so that border line of majority(2/3) is same to that of minority (1/3)*2
    // Then we sweep the array again to verity if these candidates are really majorities.
    // Notice: The algorithm works even if only one of two candidates is real majority.
    // To understand this situation, think of the condition that the real majority gets a down vote.
    // It won't happen more than 1/2 of the rest votes, counter2 need to 
    vector<int> majorityElement(vector<int>& nums) {
        vector<int> result;
        int n = nums.size();
        if(n == 0) return result;
        int major1 = 0, major2 = 1; // Initialize to arbitary numbers
        int count1 = 0, count2 = 0;
        for(int i = 0; i < n; i++){
            if(nums[i] == major1){
                count1++;
            }
            else if(nums[i] == major2){
                count2++;
            }
            else if(count1 == 0){ // We change orders of if statement to eliminate duplicates
                major1 = nums[i];
                count1 = 1;
            }
            else if(count2 == 0){
                major2 = nums[i];
                count2 = 1;
            }
            else{
                count1--;
                count2--;
            }
        }
        // Verify majority
        // Reset counters
        count1 = 0;
        count2 = 0;
        for(int i = 0; i < n; i++){
            if(nums[i] == major1) count1++;
            if(nums[i] == major2) count2++;
        }
        if(count1 > n/3) result.push_back(major1);
        if(count2 > n/3) result.push_back(major2);
        return result;
    }
};
```


