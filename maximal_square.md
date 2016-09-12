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





###Optimization


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

*Optimized, DP Using 1D Array*

```

```