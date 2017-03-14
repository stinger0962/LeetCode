Given a matrix ofmxnelements \(mrows,ncolumns\), return all elements of the matrix in spiral order.

For example,  
Given the following matrix:

```
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]

```

You should return`[1,2,3,6,9,8,7,4,5]`.





---



The point is how to build the process elegantly and simple. \(without annoying corner case\)



```
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result;
        if (matrix.empty())
            return result;
        int rowBegin = 0;
        int rowEnd = matrix.size() - 1;
        int colBegin = 0;
        int colEnd = matrix[0].size() - 1;
        
        // repeatedly traverse right->down->left->up 
        while (rowBegin <= rowEnd && colBegin <= colEnd) {
            for (int i = colBegin; i <= colEnd; i++)
                result.push_back(matrix[rowBegin][i]);
            ++rowBegin;
            
            for (int j = rowBegin; j <= rowEnd; j++)
                result.push_back(matrix[j][colEnd]);
            --colEnd;
            
            // consider corner case, only one row/col left when enter while loop
            if (rowBegin <= rowEnd) {
                for (int i = colEnd; i >= colBegin; i--)
                    result.push_back(matrix[rowEnd][i]);
                --rowEnd;
            }
            if (colBegin <= colEnd) {
                for (int j = rowEnd; j >= rowBegin; j--)
                    result.push_back(matrix[j][colBegin]);
                ++colBegin;
            }
        }
        
        return result;
    }
    
};
```



