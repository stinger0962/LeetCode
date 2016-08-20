# Distinct Subsequences


Given a string S and a string T, count the number of distinct subsequences of T in S.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, ```"ACE"``` is a subsequence of ```"ABCDE"``` while ```"AEC"``` is not).

Here is an example:
```S = "rabbbit", T = "rabbit"```

Return ```3```.



The discription craps. In plain English, it asks for **how many ways do we have to generate T by only REMOVING, but not reordering chars in S**?

In the example, we can **remove** any one of the three ```'b'``` to generate "rabbit", so the answer is ```3```.


---



###Maze-like Dynamic Programming -- 2D Array

This problem belongs to the same category as *11.3 Regular Expression Matching*.

The maze-like problems generally use a two-dimensional array to record states of different sizes.

We define a **2D array C[m][n] to be the solution**, number of distinct subsequences of string T with **size 0,1,...,m** in string S with **size 0,1,...,n**.


### Formula

*Note: i -- size of T, j -- size of S*


Let's derive the solution of C[i][j] in general.

1. The last char doesn't match ```T[i-1] != S[j-1]```. In this case, the last char of S will not be used to build T, thus we are building T from S without ```S[j-1]```, which brings us to ```C[i][j] = C[i][j-1]```.
2. The last char matches ```T[i-1] == S[j-1]```. We separate this case into two subcases. The solution is the **SUM** of two subcases.

  (1) The last char of S is used to construct T. We can discard the last char from both strings since the last char doesn't affect the ways of building T, which brings us to the state of ```C[i-1][j-1]``` .
  
  (2) The last char of S is *NOT* used to construct T. In other words, we need to build T from S without ```S[j-1]```, which is the state of ```C[i][j-1]```.
  
  
Adding two conditions together, we have :  

```
if  T[i-1] == S[j-1]

then C[i][j] = C[i-1][j-1] + C[i][j-1]```


### Starting Conditions

When T is empty, there is always exactly ONE way to build T from S -- removing them all.

When S is empty while T is not, there is NO way to build T from S.


###The Code

```
class Solution {
public:
    int numDistinct(string s, string t) {
        int m = t.size();
        int n = s.size();
        // Our solution is C[m][n]; also notice the order of m,n
        // C[i][j] is the number of ways to build T of size i from S of size j, using only removing, while keep the order of chars
        vector<vector<int>> C(m+1, vector<int>(n+1,0)); 
        
        // Starting Conditions
        for(int j = 0; j<=n; j++){
            C[0][j] = 1;
        }
        for(int i = 1; i <=m; i++){
            C[i][0] = 0;
        }
        // Formula
        for(int i = 1; i<=m; i++){
            for(int j = 1; j<=n; j++){
                if(t[i-1] != s[j-1]){
                    C[i][j] = C[i][j-1];
                }
                else{
                    C[i][j] = C[i-1][j-1] + C[i][j-1];
                }
            }
        }
        return C[m][n];
    }
};
```