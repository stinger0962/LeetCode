# 9.1 Longest Substring Without Repeating Characters



Given a string, find the length of the longest substring without repeating characters.


---
### Read substring reversely

The trick here is that we don't treat a substring from left to right. Inversely, we loop through the string, check the possible longest substring left to each character. 



### The O(N) Algorithm  


We use **double pointers** to solve the problem. **Between these two pointers is the longest substring** without repeating characters we are building.

1. The right pointer loops through the string. 

2. When it encounters a character C that has already appeared before, we **compare the last found position of C and the left pointer**. 

3. If C's last found position is **smaller** than the left pointer -- which means C is not part of our substring -- we can safely add C to the substring. Nothing changes. 

4. If C's last found position is **greater** than the left pointer, then C is alreayd in the substring. To avoid repeating, we move left pointer to the right of C's last found position. 

5. We update the max length of substring as necessary before the right pointer move ahead.


### The Code

With a total understanding of the fundamental concepts, the code is surprisingly concise.


```
class Solution {
public:
    int lengthOfLongestSubstring(string s) 
    {
        int maxLen = 0;
        unordered_map<char, int> pos; // Key: char in the string, value: its last found position
        for(int i = 0, j = 0; i < s.size(); i++) // Double pointers representing range of substring, i - right bound, j - left bound
        {
            char ch = s[i]; 
            if(pos.count(ch) > 0)
            {
                j = max(j, pos[ch]+1);
            }
            pos[ch] = i;
            maxLen = max(maxLen, i-j+1);
        }
        return maxLen;
    }
};
```


