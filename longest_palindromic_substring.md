# Longest Palindromic Substring

Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.



---



There is a famous, non-trivial solution for this problem, known as **Manacher's algorithm**.

A good constructed, easy to understand article about this topic can be found in the below link.

http://articles.leetcode.com/longest-palindromic-substring-part-ii/


We will probably not see a problem like this in an hour-ish interview. Yet it is still worth taking some time to read through the article (some part requires reading multiple times) and get a thorough understanding of the algorithm. 



---

```
class Solution 
{
public:
    string longestPalindrome(string s) 
    {
        string t = preProcess(s); // transform n-length string to 2n+1 length
        int n = t.length();
        int *p = new int[n];
        int c = 0, r = 0;
        for(int i = 1; i < n-1; ++i)
        {
            int i_mirror = 2 * c - i; // right bound - i = i_mirror - left bound;
            // p[i] = (r>i) ? min(r-i, p[i_mirror]);
            // The below if/else condition is same as the above one-line code, but much easier to understand
            if(p[i_mirror] < r-i) // If LPS@i_mirror doesn't expand over left bound, then LPS@i = LPS@i_mirror
            {
                p[i] = p[i_mirror];
            }
            else // If LPS@i_mirror expands beyond left bound, then LPS@i at least reaches right bound
            {
                p[i] = r-i; 
            }
            // For chars beyond right bound, we check them one by one
            while( t[i + 1 + p[i]] == t[i -1 - p[i]] )
            {
                p[i]++;
            }
            // Update c and r when LPS@i goes beyond r
            if( i + p[i] > r )
            {
                c = i;
                r = i + p[i];
            }
        }
        
        // Now we've got p[] all set, loop through p[] to retrieve the max length
        int maxLen = 0, centerIndex = 0;
        for(int i = 1; i < n-1; ++i)
        {
            if(p[i] > maxLen)
            {
                maxLen = p[i];
                centerIndex = i;
            }
        }
        delete[] p;
        
        return s.substr( (centerIndex - 1 - maxLen)/2, maxLen );
    }
    
private:
// Transform n-length string to 2n+1 by adding a # between each char, and one at each end.
// ^ and $ signs are sentinels appended to each end to avoid bounds checking, not actually part of the string
    string preProcess(string s)
    {
        int n = s.length();
        if(n==0)
        {
            return "^$";
        }
        string trans = "^";
        for(int i = 0; i < n; ++i)
        {
            trans += "#" + s.substr(i, 1);
        }
        trans += "#$";
        return trans;
    }
};
```