# Maximal Square


Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

For example, given the following matrix:

```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

Return 4.



---


###Dynamic Programming Approach

As always, dynamic programming is a good approach to solve a two dimensional space with unknown size.

The key point is to set up our 2D array with **correct meaning**.

Say we have a mxn 2D array DP. ```DP[i][j]``` represents the largest side length of a square that we can achieve with right bottom corner at ```matrix[i][j]```



The top row and the left most column are the starting status. DP is 1 if char in that location is 1, otherwise DP is 0.

Now let's talk about general ```DP[i][j]```.

It's easy to get:
```
if matrix[i][j] = 0,
    DP[i][j] = 0;
```
While it is not so easy to get: 

```
if matrix[i][j] = 1,
    DP[i][j] = min(DP[i][j-1], DP[i-1][j], DP[i-1][j-1]) + 1;
```

Grabbing a pen and draw some picture will help understand the formula. 

Remember that when finding ```DP[i][j]```, we are always looking at ```D[i-1][j], D[i-1][j-1], and D[i][j-1]``` for clues.

###Optimization

Since we will only use three cells when updating DP, we don't need to keep the entire 2D array.

A simple optimization is two keep two rows, previous and current one.

A better optimization only keep one row, and an additional variable to store DP[i-1][j-1].

The latter one has a **O(mxn) time complexity and uses O(n) extra space**.

###The Code

*DP Using 2D Array*

```
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int row = matrix.size();
        int col = matrix[0].size();
        // Max size of square we can achieve
        int maxSize = 0;
        // dp[i][j] represents the largest size of square we can achieve of which
        // right bottom corner is located at matrix[i][j]
        vector<vector<int>> dp(row, vector<int>(col, 0));
        
        // Starting status, the left most column and the top row
        for(int i = 0; i < row; i++){
            dp[i][0] = matrix[i][0] == '1' ? 1 : 0;
            maxSize = max(maxSize, dp[i][0]);
        }
        for(int j = 1; j < col; j++){
            dp[0][j] = matrix[0][j] == '1' ? 1 : 0;
            maxSize = max(maxSize, dp[0][j]);
        }
        
        // Formula
        for(int i = 1; i < row; i++){
            for(int j = 1; j < col; j++){
                if(matrix[i][j] == '1'){
                    dp[i][j] = min(min(dp[i-1][j], dp[i-1][j-1]), dp[i][j-1]) + 1;
                }
                maxSize = max(dp[i][j], maxSize);
            }
        }
        return maxSize * maxSize;
    }
};
```

*Optimized, DP Using 1D Array and one additional variable*

```
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        if(matrix.empty()){
            return 0;
        }
        int row = matrix.size();
        int col = matrix[0].size();
        // Max size of square we can achieve
        int maxSize = 0;
        // We reuse the last row to store new data
        vector<int> dp(col, 0);
        
        // Starting status, only the top row
        for(int i = 0; i < col; i++){
            dp[i]= matrix[0][i] == '1' ? 1 : 0;
            maxSize = max(maxSize, dp[i]);
        }

        // Formula
        // We will need one more variable to store the value of previous dp[i-1][j-1]
        int pre = 0;
        for(int i = 1; i < row; i++){
            for(int j = 0; j < col; j++){
                // First column
                if(j == 0){
                    pre = dp[0];
                    dp[j] = matrix[i][j] == '1' ? 1 : 0;
                }
                // Draw two rows to understand the code
                else if(matrix[i][j] == '1'){
                    int temp = dp[j];
                    dp[j] = min(min(dp[j-1], dp[j]), pre) + 1;
                    pre = temp;
                }
                else{
                    pre = dp[j];
                    dp[j] = 0;
                }
                maxSize = max(dp[j], maxSize);
            }
        }
        return maxSize * maxSize;
    }
};
```