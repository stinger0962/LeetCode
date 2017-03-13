You are given annxn2D matrix representing an image.

Rotate the image by 90 degrees \(clockwise\).

Follow up:  
Could you do this in-place?



---



```
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        // a general algorithm of in-place image rotating: 
        // rotate it in two steps: 
        // 1st swap by rows (up, down) or by cols (left, right)
        // 2nd swap by diagonals (row to col, col to row)
        int sz = matrix.size();
        for (int i = 0; i < sz / 2; ++i) {
            swap (matrix[i], matrix[sz - i - 1]); // swap two rows
        }
        for (int i = 0; i < sz; ++i) 
            for (int j = i+1; j < sz; ++j) 
                swap (matrix[i][j], matrix[j][i]);
    }
};
```



