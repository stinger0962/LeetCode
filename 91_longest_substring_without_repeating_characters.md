# 9.1 Longest Substring Without Repeating Characters



Given a string, find the length of the longest substring without repeating characters.


---

### Time Complexity: O(N) 


We use double pointers to solve the problem. Between these two pointers is the longest substring without repeating characters we are building.

The right pointer loops through the string. 

When it encounters a character C that has already appeared before, we compare the last found position of C and the left pointer. 

If C's last found position is smaller than the left pointer -- which means C is not part of our substring yet -- we can safely add C to the substring. Nothing changes. 


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


