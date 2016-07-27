# Isomorphic Strings

Given two strings s and t, determine if they are isomorphic.

Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

For example,
Given "egg", "add", return true.

Given "foo", "bar", return false.

Given "paper", "title", return true.



---


A straightforward attempt is to traverse both strings char by char, compare them to see if they meet the constraints. 

One thing worths attention is that isomorphic strings are two-way replacements. 

**Two strings mirror each other**. **Thus any comparison made from s to t must pair with a same comparison from t to s.**

An intuitive solution uses two hashmaps (unordered_map) to store pairs of chars, one pointing from s to t, the other from t to s. 



---

```
class Solution {
public:
    bool isIsomorphic(string s, string t) 
    {
        // In this two-way replacement, set up two hashtables to oversee invalid transforms
        unordered_map<char, char> stot; // Key is char in s, while value is char in t
        unordered_map<char, char> ttos; // Key is char in t, while value is char in s
        bool isSor = true;
        
        for(int i = 0; i < s.size(); ++i)
        {
            if( stot.count(s[i]) == 0 && ttos.count(t[i]) == 0)
            {
                stot[s[i]] = t[i];
                ttos[t[i]] = s[i];
            }
            else if( stot[s[i]] != t[i] || ttos[t[i]] != s[i]) // Invalid
            {
                isSor = false;
                break;
            }
        }
        return isSor;
    }
};
```



#Performance# 

We know that a hashtable/map performs better than an array in terms of searching time. This is true in most of time. Yet **when we know the index, searching in an array is faster than that in a hashtable**. The reason behind is that hashtable uses an underlining array of linked list, not to mention the hash function. 


> When dealing with string char by char, we can take the advantage that each char can be converted to an integer, **wrapping all the chars into an array of integer[256] (or int[128] )** to achieve faster access than using a hashtable.

Below is a second/better solution using array instead of hashtable. As we can see, the first solution has a run time of **36ms**, while the second one finishes at** 8ms**.



---

```
class Solution {
public:
    bool isIsomorphic(string s, string t) 
    {
        // Using array(vector) instead of hashtable
        vector<char> stot(127, 0); // index: char in s; value: char in t
        vector<char> ttos(127, 0); // index: char in t; value: char in s
        bool isSor = true;
        
        for(int i = 0; i < s.size(); ++i)
        {
            if( stot[s[i]] == 0 && ttos[t[i]] == 0)
            {
                stot[s[i]] = t[i];
                ttos[t[i]] = s[i];
            }
            else if( stot[s[i]] != t[i] || ttos[t[i]] != s[i]) // Invalid
            {
                isSor = false;
                break;
            }
        }
        return isSor;
    }
};
```






