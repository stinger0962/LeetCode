# The Skyline Problem


A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are given the locations and height of all the buildings as shown on a cityscape photo (Figure A), write a program to output the skyline formed by these buildings collectively (Figure B).


![](https://leetcode.com/static/images/problemset/skyline1.jpg)

![](https://leetcode.com/static/images/problemset/skyline2.jpg)


 Buildings Skyline Contour
The geometric information of each building is represented by a triplet of integers ```[Li, Ri, Hi]```, where Li and Ri are the x coordinates of the left and right edge of the ith building, respectively, and Hi is its height. It is guaranteed that ```0 ≤ Li, Ri ≤ INT_MAX, 0 < Hi ≤ INT_MAX, and Ri - Li > 0```. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

**Input: **For instance, the dimensions of all buildings in Figure A are recorded as: ```[ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ]```.

The output is a list of "**key points**" (red dots in Figure B) in the format of ```[ [x1,y1], [x2, y2], [x3, y3], ... ]``` that uniquely defines a skyline. 

**A key point is the left endpoint of a horizontal line segment**. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.

**Output**: For instance, the skyline in Figure B should be represented as:```[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ]```. (**All left point of horizonal line, as well as the right most point**)

**Notes**:

The number of buildings in any input list is guaranteed to be in the range [0, 10000].

The input list is already sorted in ascending order by the left x position Li.

The output list must be sorted by the x position.

There must be no consecutive horizontal lines of equal height in the output skyline.

For instance, ```[...[2 3], [4 5], [7 5], [11 5], [12 7]...]``` is not acceptable; the three lines of height 5 should be merged into one in the final output as such: ```[...[2 3], [4 5], [12 7], ...]```





---




### Analysis of the Requirement

Fisrt, this is a complicated, integrated problem. It doesn't ask a single aspect of programming, but a combination of a set of knowledge. 

Let's first tackle down the long text into simple question. What does this problem really ask for?

In short, we are looking for a collection of critical points which are vectice of buildings.

Let's transform the context into the following pseudocode.

```
for each critical point C (time N)
    C.height = height of the tallest building over C  (time LogN)
    not including the building's right edge
```

### $$NLogN$$ Algorithm

Straightforwardly, finding tallest building requires some structure like **heap**. 

Main steps are as followings, see comments for treatments of special conditions. 

```
1. Sort CP(critical points) by their x coordinates, categorize them into two groups, left and right.

2. Scan CP from left to right.

3. When encountering left vertex, add the building into heap, CP's height = tallest in heap

4. When encountering right vertex, remove the building from heap, CP's height = tallest in heap

```

###Custom Sort

We will store CP's info in a pair value. First is its X, and second is its height.

Height of a left vertex is **positive**, while that of a right vertex is **negative**.

We will want to sort our pairs by their x values, ascendingly. 

If two points share same x, we sort them by their height, descendingly. That is, left vertex comes before right points. (We come up to this sort from debugging.)

###Appropriate Data Structure

Ideally, we want to use STL's PQ(priority queue) to implement heap. However, PQ doesn't support looking up, which we will want to use to remove a certain building from the heap. 

Alternatively, we will use a **multiset** to mimic the behavior of a heap.

```multiset.rbegin()``` will return a pointer (which we can dereference to get the value) to the largest member in the set.

```multiset.erase(multiset.find("key"))``` will remove a single entry with the key value.



###The Code

```
class Solution {
public:
    // We need a collection of critical points C
    // For each critical point C, set C.Y to the tallest rectangle over c(right edge excluded)
    // Omit duplicate height when necessary
    vector<pair<int, int>> getSkyline(vector<vector<int>>& buildings) {
        // Main steps:
        // 1. Sort the CP, group them as left and right edge
        // 2. Scan CP from left to right. There are two conditions.
        // 3. When encounting left edge, add rectangle to the heap, height being key
        //      then set CP.Y to the tallest height in heap
        // 4. When encounting right edge, remove rectangle from the heap 
        //      then set CP.Y to the tallest height in heap
        
        vector<pair<int, int>> result;
        vector<pair<int, int>> cps; // Collection of categorized and sorted critical points
        for(const auto& rec: buildings){
            cps.push_back(make_pair(rec[0], rec[2])); // Left vertex has possible y value
            cps.push_back(make_pair(rec[1], -rec[2])); // Right vertex has negative y value
        }
        
        
        
        sort(cps.begin(), cps.end(), sortCP);
        // **** Design Change -- Use Multiset instead of Priority Queue
        // If we use priority queue to store height of active rectangles
        // We will not ba able to delete a specific height associated with right vertex
        // The reason behind is that priority queue doesn't support such action.

        // By default, elements in Multiset are sorted ascendingly,
        // which allow us access the last element to retrieve the tallest height currently active
        multiset<int> activeHeight; // The collection of height of all active rectangles
        
        
        for(const auto& cp : cps){
            if(cp.second > 0){ // Left vertex
                activeHeight.insert(cp.second);
            }
            else{ // Right vertex
                // Be aware NOT to erase multiple heights with same value!
                // multiset.find() points to a single element
                activeHeight.erase(activeHeight.find(-cp.second)); 
            }
            int xCoord = cp.first;
            // Height is zero if there is no avtive building 
            int height = activeHeight.empty() ? 0 : *activeHeight.rbegin();
            // To avoid consecutive same height, we compare this value to the last element in the result
            if(result.empty() || (result.rbegin()->second != height && result.rbegin()->first != xCoord) ){
                result.push_back(make_pair(xCoord, height));
            }
        }
        return result; 
    }
    
private:
        // Sort critical points by x-coordinate ascendingly
        // For points having same x-coordinate, left and higher vertex comes first
        bool static sortCP(pair<int, int> a, pair<int, int> b){
            if(a.first == b.first){
                return a.second > b.second;
            }
            else{
                return a.first < b.first;
            }
        }
};
```
