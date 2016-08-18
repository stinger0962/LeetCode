# Regular Expression Matching

Implement regular expression matching with support for ```'.'``` and ```'*'```.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true```


**Clarification** : ```'*'``` doesn't appear alone. It pairs with the preceding character, meaning the preceding character can appear **ZERO** or more times. 

(e.g.) ```"c*"``` means ```empty, c, cc, ccc, ...```


---


###Dynamic Size ? Try Dynamic Programming


Dynamic programming is a terrific technique to **solve problems with undetermined size**. 

In this problem, the size of both strings are unknown. Let's denote them as M and N. Dynamic programming is thus to get the solution of **(M,N)** from the solution of **(M-1, N-1)**. 

