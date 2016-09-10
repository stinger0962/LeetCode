# Triangle

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle
```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]```
The minimum path sum from top to bottom is ```11``` (i.e., 2 + 3 + 5 + 1 = 11).



---

###Analysis

First, let's define a two dimensional array to represents nodes in the triangle.

a[i][j] represents a node at row i and column j. Both i and j are zero based.

Our goal is to find the min sum to bottom of node a[0][0].

Intuitively, we can write the following equation:


```a[0][0] = min(a[1][0], a[1][1]) + a[0][0]```

Follow this pattern, we can find the **formular** we need for any node a[i][j] in the triangle

```a[i][j] = min(a[i+1][j], a[i+1][j+1]) + a[i][j]```

The **starting status** is the nodes in the bottom, of which min sum equal to themselves.

###Bottom-up DP

Recall that DP has two patterns, top-down and bottom-up.

Bottom-up DP starts building up solution from starting status, our solution is the last one in the sequence.

Inversely, top-down DP starts from the solution we want, the starting status is the last one in the sequence.

In this problem we will not get to a[0][0] until last step. Thus It is a Bottom-up DP.



###The Code

**Solution Using O(N*N) Extra Space**

```
class Solution {
public:
    // Bottom-up dynamic programming
    // Starting from the bottom row, for each node, calculate the min sum from it to the bottom
    // Then minSum[0][0] is the accumulated min sum from bottom to top.
    int minimumTotal(vector<vector<int>>& triangle) {
        
        // Number of total rows
        int rows = triangle.size();
        // Define a 2D array, each cell storing a node's min sum to the bottom
        vector<vector<int>> minSum{rows, vector<int>(rows+1)};
        
        // Starting states: 
        // Last row's min sum to the bottom is the value of the node

        for(int i = 0; i < triangle.back().size(); i++){
            minSum[rows-1][i] = triangle[rows-1][i];
        }
        // Calculate each node's min sum to the bottom
        // Optimization: instead of 2D array, we reuse the 1D array
        for(int row = rows - 2; row >=0; row--){
            for(int col = 0; col <= row; col++){
                minSum[row][col] = min(minSum[row+1][col], minSum[row+1][col+1]) + triangle[row][col];
            }
        }
        return minSum[0][0];
    }
};
```


**Optimizied Solution Using O(N) Extra Space**

```
class Solution {
public:
    // Bottom-up dynamic programming
    // Starting from the bottom row, for each node, calculate the min sum from it to the bottom
    // Then minSum[0][0] is the accumulated min sum from bottom to top.
    int minimumTotal(vector<vector<int>>& triangle) {
        vector<int> minSum(triangle.back().size());
        
        // Starting states: 
        // Last row's min sum to the bottom is the value of the node
        for(int i = 0; i < triangle.back().size(); i++){
            minSum[i] = triangle[triangle.size()-1][i];
        }
        // Calculate each node's min sum to the bottom
        // Optimization: instead of 2D array, we reuse the 1D array
        for(int row = triangle.size() - 2; row >=0; row--){
            for(int col = 0; col <= row; col++){
                minSum[col] = min(minSum[col], minSum[col+1]) + triangle[row][col];
            }
        }
        return minSum[0];
    }
};
```