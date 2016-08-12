# House Robber



You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine **the maximum amount of money you can rob tonight without alerting the police**.



---


###Dynamic Programming

See the **chapter summary** for more about DP.


> The two essential elements of DP are a formular and a starting status. 



Let F(n) be the maximum amount for robbing n houses, and money(i) be the money in i-th mouse. 

With some observation, we can come into the following **formula** : 

F(n) = max (F(n-2) + money(n), F(n-1) )

Our **starting status** is F(1) = money(1); F(2) = max (money(1), money(2) );


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
        if(nums.size() == 2){
            return max(nums[0], nums[1]);
        }
        vector<int> money(nums.size()); // a data structure storing solution for all subproblems
        money[0] = nums[0];
        money[1] = max(nums[0], nums[1]); // [0] and [1] are starting status
        for(int i = 2; i < nums.size(); i++){
            money[i] = max(money[i-2]+nums[i], money[i-1]); // the formular 
        }
        return money[nums.size()-1]; 
    }
};
```