# Minimum Path Sum

Given a ```m x n``` grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.




---


###Intuitive DP Solution

If we create a 2D array, of which each cell represents the min sum from top left to its position, we will notice that ```minSum[i][j] = min(minSum[i][j-1], minSum[i-1][j]) + grid[i][j]```

Thus we can build an intuitive DP solution based on the formula.

We can further optimize the solution by reducing the 2D array to 1D array.

We provide both solutions as below.

###The Code

*2D Array DP* --  O(M*N) Extra Space

```
class Solution {
public:
    // Dynamic programming
    // Create a 2D array, each cell representing the min sum from top left to the cell
    // Intuitively, minSum[i][j] is the min of minSum[i-1][j] and minSum[i][j-1] plus the grid[i][j]
    int minPathSum(vector<vector<int>>& grid) {
        if(grid.size() == 0) return 0;
        int height = grid.size();
        int width = grid[0].size();
        vector<vector<int>> minSum(height, vector<int>(width, 0)); // Pay attention to the way of initializion
        minSum[0][0] = grid[0][0];
        for(int i = 1; i < height; i++){
            minSum[i][0] = minSum[i-1][0] + grid[i][0]; 
        }
        for(int j = 1; j < width; j++){
            minSum[0][j] = minSum[0][j-1] + grid[0][j];
        }
        for(int i = 1; i < height; i++){
            for(int j = 1; j < width; j++){
                minSum[i][j] = min(minSum[i-1][j], minSum[i][j-1]) + grid[i][j];
            }
        }
        return minSum[height-1][width-1];
    }
};
```

Optimization: 1D Array DP --  O(M) Extra Space

```
class Solution {
public:
    // Dynamic programming with optimization
    // Use a 1D array instead of 2D array
    int minPathSum(vector<vector<int>>& grid) {
        if(grid.size() == 0) return 0;
        int height = grid.size();
        int width = grid[0].size();
        vector<int> minSum(width, 0); // Optimization: use 1D array instead
        minSum[0] = grid[0][0];
        for(int j = 1; j < width; j++){ // Top line
            minSum[j] = minSum[j-1] + grid[0][j];
        }
        for(int i = 1; i < height; i++){ // The rest of the grid
            for(int j = 0; j < width; j++){ 
                if(j == 0) minSum[j] = minSum[j] + grid[i][j]; // Tackle first column specially
                else minSum[j] = min(minSum[j-1], minSum[j]) + grid[i][j];
            }
        }
        return minSum[width-1];
    }
};
```