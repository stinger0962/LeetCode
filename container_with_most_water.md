# Container With Most Water

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container.

For example: if input is 1,2,1, then output is 2 (we choose line #1 and line #3).

---


###Double Pointers

The problem is to find extreme range that meets certain conditions. Using double pointers is the best way to solve this kind of problems. 

**Double pointers do not necessary both move forward.**

In this problem, one pointer starts from the left bound, with the other from the right. They **move towards each other**.  


###Idea Behind

Starting from evaluating the widest container, if any "narrow" container holds more water, it must have a larger height. 

Thus, we can removes lines from both sides that don't support a larger height. The process repeats until no more containers remain.

###The Code


```
class Solution {
public:
    int maxArea(vector<int>& height) {
        int mostWater = 0;
        int i = 0;
        int j = height.size()-1;
        while(i < j){
            int h = min(height[i], height[j]); // the height of the container comprised by line i and j
            mostWater = max(mostWater, (j - i) * h);
            // Remove lines from both sides that don't support a larger height
            while(height[i] <= h && i < j){
                i++;
            }
            while(height[j] <= h && i < j){
                j--;
            }
        }
        return mostWater;
    }
};
```