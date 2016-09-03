# Pascal's Triangle

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return

```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]```


---


### A Neat Solution

As for an easy to implement problem, we try to implement it elegantly.

The inspiring idea is that we can call vector.resize() function to dynamically set each row's size.

```
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> pascal(numRows); // Specify the size of rows
        // It takes some good idea to integrate the first two rows into general conditions
        for(int i = 0; i < numRows; i++){
            pascal[i].resize(i+1); // Specify the size of columns of each row
            pascal[i][0] = pascal[i][i] = 1; // Fill in two ends
            for(int j = 1; j < i; j++){
                pascal[i][j] = pascal[i-1][j] +pascal[i-1][j-1];
            }
        }
        return pascal;
    }
};
```


### Pascal's Triangle II

Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,
Return ```[1,3,3,1].```




---


```
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<vector<int>> pas(rowIndex+1);
        for(int i = 0; i <= rowIndex; i++){
            pas[i].resize(i+1);
            pas[i][0] = pas[i][i] = 1;
            for(int j = 1; j < i; j++){
                pas[i][j] = pas[i-1][j] + pas[i-1][j-1];
            }
        }
        return pas[rowIndex];
    }
};
```
