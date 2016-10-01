# Smallest Rectangle Enclosed Black Pixel

An image is represented by a binary matrix with 0 as a white pixel and 1 as a black pixel. **The black pixels are connected**, i.e., there is only one black region. 

Pixels are connected horizontally and vertically. Given the location ```(x, y)``` of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.

For example, given the following image:

```[
  "0010",
  "0110",
  "0100"
]```
and ```x = 0, y = 2,```
Return 6.



---


###How to Come up with Binary Search

It is not intuitive that what algorithm we should use. We have some job to do before we can come up with the idea of using binary search.

Firstly, we are looking for boundary black pixels (To simplify, let's call it "1") in four directions. It is not easy to search for specific cells in a 2D array. It will be much easier if we can compress 2D into 1D. 

Here is how to convert 2D into 1D. Let's **project all the table onto bottom row** in a way that if a column contains any "1", the corresponding cell in bottom row is also "1". Otherwise it is "0".

We can make an important observation that **the "1" cells on the project should be continuous**, i.e., **there is only one black region on the project**. 

We can **prove** the hypothesis as below: if there are two black regions on the project, choose the "0" cell between two black regions. The column contains the "0" cell will divide the table into two, each containing a black region, which is contradict with the problem statement.

Our task is thus to find the left and right boundary of the a sequence of continuous "1" in a onedimension array. The array is sorted in both sides, hence we can apply binary search. 


> Find boundary of "1" in 0000111111111111100000000000

###Tricky Maneuvers in Implementation

Even though we have come up with the proper algorithm, we still have to tackle several hazards in the implementation.

To be concise, there are two notable maneuvers.

1. Beginning index inclusive and ending index exclusive is adopted in the algorithm. (e.g.) We search for index of boundary 1 in the left part, and index of boundary 0 in the right part in the **same function**. 

2. To implement this asymmetric function beautifully, we will use some **uncommon combination of logical operators**.

3. For further **optimization**, We use two additional parameters to limit the range of scanning when projecting pixels.

Details are provided in the comments.

###The Code

```
class Solution {
public:
    int minArea(vector<vector<char>>& image, int x, int y) {
        int rows = image.size();
        if (rows == 0) return 0;
        int cols = image[0].size();
        int top = searchProjCol(image, 0, x, 0, cols, true);
        int bot = searchProjCol(image, x+1, rows, 0, cols, false);
        int left = searchProjRow(image, 0, y, top, bot, true);
        int right = searchProjRow(image, y+1, cols, top, bot, false);
        return (bot - top) * (right - left);
    }
    // Search projected row for the index of left bounding black pixel, and the index of right bounding white pixel on a projected row
    // Difference of two indexes is the width of rectangle
    // image: the given matrix
    // i: left cursor
    // j: right cursor
    // top: upper boundary of searching range(inclusive) when projecting
    // bot: lower boundary of searching range(exclusive) when projecting
    // top and bot are used for optimization. They reduce unnecessary scans.
    int searchProjRow(vector<vector<char>>& image, int i, int j, int top, int bot, bool isLeft){
        // Binary search finishes when two cursors overlap
        while(i != j){
            int mid = (i+j) / 2;
            // Scan the column with index of mid
            // If any '1' found, then project '1', otherwise project '0'
            int k = top; 
            while(k < bot && image[k][mid] != '1') k++; 
            // Searching left bounding 1 : i......m.....j
            // When mid is '0',                    ^
            // Move i to the right of mid          i
            // Because finally we want i to be 1
            
            // Searching left bounding 1: i.....m....j
            // When mid is '1',                 ^                
            // Move j to mid                    j
            // Reason is same as above, we want to return index of 1
            
            // Scenario of finding right boundings are opposite
            
            // Searching right bounding 0 : i......m.....j
            // When mid is '0',                    ^
            // Move j to mid                       j
            // Because finally we want j to be 0
            
            // Searching right bounding 0 : i......m.....j
            // When mid is '1',                     ^
            // Move i to the right of mid           i
            // Again, we want i to be the leading 0
            
            // As we can see, there are four combinations in total (mid 0 or 1, search left or right)
            // We use a tricky maneuver to reduce four conditions into two
            if(k < bot == isLeft){ // Note the logical operator is == . while not && ||
                j = mid;
            }
            else{
                i = mid + 1;
            }
        }
        return i;
    }
    // Search projected column for the index of upper bounding '1', and the index of lower leading '0'
    // Their difference is the height of the rectangle
    // left: left boundary of searching range(inclusive) when projecting
    // right: right boundary of searching range(exclusive) when projecting
    int searchProjCol(vector<vector<char>>& image, int i, int j, int left, int right, bool isUpper){
        while(i != j){
            int mid = (i+j) / 2;
            int k = left; 
            while(k < right && image[mid][k] != '1') k++; 
            if(k < right == isUpper){
                j = mid;
            }
            else{
                i = mid + 1;
            }
        }
        return i;
    }
};
```