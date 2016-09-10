# Best Time to Buy and Sell Stock I

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

**Example 1:**
```
Input: [7, 1, 5, 3, 6, 4]
Output: 5
```

max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)

**Example 2:**
```
Input: [7, 6, 4, 3, 1]
Output: 0
```
In this case, no transaction is done, i.e. max profit = 0.


###Editorial Solution

The points of interest are the peaks and valleys in the given graph. We need to find the largest peak following the smallest valley. We can maintain two variables - minprice and maxprofit corresponding to the smallest valley and maximum profit (maximum difference between selling price and minprice) obtained so far respectively.


###The Code

```
class Solution {
public:
    // Top-down DP
    int maxProfit(vector<int>& prices) {
        int minPrice = INT_MAX;
        int maxProfit = 0;
        for(int i = 0; i < prices.size(); i++){
            if(prices[i] < minPrice){
                minPrice = prices[i];
            }
            else if(prices[i] - minPrice > maxProfit){
                maxProfit = prices[i] - minPrice;
            }
        }
        return maxProfit;
    }
};
```