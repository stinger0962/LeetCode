# Best Time to Buy and Sell Stock I, II, III

###I

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


###O(N) Time Complexity Algorithm

As always, we want to achieve the best possible algorithm. In this problem, it's O(N) since each element has to be visited at least once.

While we traversal the array, we use** two variables** to find the max profit so far.

One variable stores the **min price** so far, the other one stores the **max profit**.

```
For each N[i], 
  if N[i] is less than min price, update the min price
  if (N[i] - min price) greater than the max profit, update the max profit
              
```



###The Code

```
class Solution {
public:
    // Top-down DP
    int maxProfit(vector<int>& prices) {
        int minPrice = INT_MAX;
        int maxProfit = 0;
        for(int i = 0; i < prices.size(); i++){
            minPrice = prices[i] < minPrice ? prices[i] : minPrice;
            maxProfit = prices[i] - minPrice > maxProfit ? prices[i] - minPrice : maxProfit;
        }
        return maxProfit;
    }
};
```



---


###II

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, **you may not engage in multiple transactions at the same time** (ie, you must sell the stock before you buy again).


###Greedy Algorithm

There is not too much to say about this problem.

A greedy algorithm works for this problem. Grabbing all local profit is same as grabbing the max total profit.

We can prove this by drawing a graph with x-day and y-price. 

###The Code

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() < 2){
            return 0;
        }
        int minPrice = prices[0];
        int maxProfit = 0;
        // Simple greedy algorithm works for this problem
        // Draw a graph and we will see why
        for(int i = 1; i < prices.size(); i++){
            if(prices[i] <= prices[i-1]){
                continue;
            }
            else{
                maxProfit += prices[i] - prices[i-1];
            }
        }
        return maxProfit;
    }
};
```