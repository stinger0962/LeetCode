# House Robber II(Cycle)

After robbing those houses on that street, the thief has found himself a new place for his thievery so that he will not get too much attention. This time, all houses at this place are **arranged in a circle**. That means the first house is the neighbor of the last one. Meanwhile, the security system for these houses remain the same as for those in the previous street.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.




---

###Composite Dynamic Programming

This problem is an extension to *11.1 House Robber*. We can treat the houses as a line with one more constraint -- the first house(1st) and the last house (n-th) cannot be robbed at the same night. 

In other words, **we run two DP simultaneously**, and pick the best solution of these two.

```
Condition 1 : If we rob 1st house
                We calculate F(n-1) instead of F(n)
Condition 2 : If we skip 1st house
                We calculate F(n)

Max money is max(F(n-1) of con1, F(n) of con2)
```

The **formula** for the two conditions stay same (ref to *11.1 House Robber*), but the **starting status** are different.


###The Code

```
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.empty()){
            return 0;
        }
        if(nums.size() == 1){
            return nums[0];
        }

        
        vector<int> robFirst(nums.size()); // solutions of subproblems with condition -- rob the 1st house
        vector<int> skipFirst(nums.size()); // solutions of subproblems with condition -- skip the 1st house
        // starting status of two conditions are different
        robFirst[0] = nums[0];
        robFirst[1] = nums[0];

            
        skipFirst[0] = 0;
        skipFirst[1] = nums[1];

         
        for(int i = 2; i < nums.size(); i++){
            // robFirst[i] = robFirst[i-1], thus we use formula of robFirst[i-1]
            robFirst[i] = max(robFirst[i-2] + nums[i], robFirst[i-1] ); 
            skipFirst[i] = max(skipFirst[i-2] + nums[i], skipFirst[i-1] );
        }
        // Compare the solution of each condition and return the greater one
        return max(robFirst[nums.size()-2], skipFirst[nums.size()-1] );
    }
};
```



