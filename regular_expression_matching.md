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

In this problem, the size of both strings are unknown. Let's denote them as M and N. Using dynamic programming, we try to get the solution of **(M,N)** from the solution of **(M-1, N-1)**. 


### Two Dimensional Array

Let's denote the **word with S of size i**, and the **regular expression with P of size j**.

Since both i and j are unknown, we use a **dynamic 2D array M[i][j]** to store if S[0...i-1] matches P[0...j-1]. 

Now our dynamic programming is to get the solution of M[i][j] from that of M[i-1][j-1].

### The Formula

There are two conditions regarding the last element of P.

1. P doesn't end with ```'*'```, M[i][j] is true if M[i-1][j-1] is true and the S[i-1] == P[j-1]


2. P ends with '*' 


