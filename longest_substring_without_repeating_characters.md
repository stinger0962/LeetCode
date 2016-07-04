# Longest Substring Without Repeating Characters



We can do a brutal search with time complexity of O(nxn). We can also achieve a linear search time by introducing  **double pointers** to restrict the search range for **LSRC** (*longest substring without repeating characters*) when scanning through the string. 

Let's call these two pointers **left** and **right**. 

The right bound is straightforward. It's the index of current character in loop. 

The trick is in the left bound. We manage the left pointer in a way that **characters between the left and right pointers are unique. **

When scanning through the string, we use a **hashtable** (key : char, value : int) to keep track of **the largest index of each character**. 


> If frequent search is all that we need, **std::unordered_map** is generally better than std::map in implementation speed.

Assuming we are scanning **C**, and the length of LSRC up to now is **max**. Regarding C, there are **three conditions**.

 1. C is **not** in the hashtable. The new LSRC is thus [left, right].
 2. If c is already in the hashtable, and its value (largest index) **i[c] >= left**. We need to update left to i[c]+1 otherwise C will appear twice in [left, right].
 3. If c is already in the hashtable, but its value (largest index) i**[c] < left**. In this case there is no C in [left, right). There is no need to move left.
 
 After dealing with all of the three conditions, we have a possible new LSRS [left, right]. We compare its length with max, and update max if necessary.
 
 This procedure continues to the end of the string. 
 



---


```
class Solution // (abbr.)LSRC : longest substring without repeating characters
{
public:
    int lengthOfLongestSubstring(string s) 
    {
        if(s.length() == 0)
        {
            return 0;
        }
        unordered_map<char,int> charLargestIndex;
        int maxLength = 0; // the length of current LSRC
        int substringStartIndex = 0; // the starting index of current LSRC
        // scan the string from left to right 
        // find the length of LSRC each time adding a char
        // replace the length of LSRC if it's larger than the current length of LSRC
        for(int i = 0; i < s.length(); ++i)
        {
            // do something when next char has already appeared in the hashtable
            if(charLargestIndex.find(s[i]) != charLargestIndex.end()) 
            {
                int nextCharIndex = charLargestIndex[ s[i] ];
                // There are two cases
                // case 1: next char is not part of the current LSRC, do nothing
                // case 2: next char appears in the current LSRC, do something
                if(nextCharIndex > substringStartIndex - 1) 
                {
                    substringStartIndex = nextCharIndex + 1; // From this moment, the starting index of LSRC has to change, otherwise the next char will appear twice in LSRC 
                     // maxLength = i - substringStartIndex + 1;
                }
            }
            charLargestIndex[s[i]] = i;
            // now compare two candidates: 
            // the old LSRC,
            // and the possible new one: starting from possible new index, ending at next char (inclusive)
            // to get the new length of LSRC
            maxLength = (i - substringStartIndex + 1 > maxLength) ? i - substringStartIndex + 1 : maxLength;
        }
        return maxLength;
    }
};```