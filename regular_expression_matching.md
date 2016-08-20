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

1. P doesn't end with ```'*'```, M[i][j] is true if M[i-1][j-1] is true and the last char matches. ```S[i-1] == P[j-1]```


2. P ending with ```'*'``` is a OR combination of the following two conditions. Either matches, then P matches S.

  (1) ```'*'``` means the preceding character appears zero times. In this case, M[i][j] is true if M[i][j-2] is true.
  
  (2) The preceding character appears at least once. A critical observation is that in this case ```"X*"``` equals to ```'X*X'```. 
  
  The transformation brings us back to condition 1. In other words, M[i][j] == M[i][j+1], where P doesn't end with ```'*'```.  M[i][j+1] is true if M[i-1][j] is true, and their last char matches. ```S[i-1] == P[j]```. We also notice that ```P[j-2] == P[j]```. Thus we have ```S[i-1] == P[j-2]```.
  

Also don't forget '.' can replace any single characters. 

 
 ### The Starting Conditions
 
Starting conditions are similar to that of building a maze.

We start from ```M[0][0] == true```, where empty strings matches each other.

Then we consider two border lines, that is, either i or j is zero.

M[0][j] is true if and only if P follows the pattern of ```"X*X*X*..."``` where all "X*" means empty.

Writing in formula, we have ```M[0][j] = ( j > 1 && P[j-1] =='*' && M[0][j-2] == true)```

M[i][0], where i > 0, are always false.


###The Code

Putting them together, the code is as below. We also provide a second solution using recursion, but the running time of 2nd solution is 700ms, comparing to 40ms of DP.

**DYNAMIC PROGRAMMING**
```
class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.size();
        int n = p.size();
        // M[i][j] stores isMatch boolean value for string s,p of size i,j
        vector<vector<bool>> M(m + 1, vector<bool>(n+1, false));  // m+1, n+1, because M[m][n] is our solution
        
        // Starting Conditions
        M[0][0] = true;
        for(int i = 1; i <= m; i++){
            M[i][0] = false;
        }
        for(int j = 1; j <= n; j++){
            M[0][j] = j > 1 && p[j-1] == '*' && M[0][j-2];
        }
        // Formula
        for(int i = 1; i <= m; i++){
            for(int j = 1; j <=n; j++){
                if(p[j-1] != '*'){
                    M[i][j] = M[i-1][j-1] && (s[i-1] == p[j-1] || '.' == p[j-1]);
                }
                else{ // p ends with '*'
                    M[i][j] = (M[i][j-2]) || ( M[i-1][j] && ( s[i-1] == p[j-2] || '.' == p[j-2]));
                }
            }
        }
        return M[m][n];
    }
};
```



---

**RECURSION**


```
class Solution {
public:
    bool isMatch(string s, string p) {
        if(p.empty()){
            return s.empty();
        }
        if(s.empty()){
            return p.empty() || (p.size() ==2 && p[1] == '*');
        }
        if(p[1] == '*'){
            if(s[0] != p[0] && p[0] != '.'){
                return isMatch(s, p.substr(2));
            }
            else{
                return isMatch(s.substr(1), p);
            }
            // return isMatch(s, p.substr(2)) || ( !s.empty() && (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p) );
        }
        else{
            if(s[0] == p[0] || p[0] == '.'){
                return isMatch(s.substr(1), p.substr(1));
            }
            else{
                return false;
            }
            // return !s.empty() && (s[0] == p[0] || p[0] == '.') && isMatch(s.substr(1), p.substr(1));
        }
    }
};
```

