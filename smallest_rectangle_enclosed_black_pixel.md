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

1. We search for index of boundary 1 in the left part, and index of boundary 0 in the right part in the **same function**. Difference of these indexes is the width of the target rectangle.

2. To implement this asymmetric function beautifully, we will use some **uncommon combination of logical operators**.

Details are provided in the comments.

###The Code

