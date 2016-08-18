# Distinct Subsequences


Given a string S and a string T, count the number of distinct subsequences of T in S.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, ```"ACE"``` is a subsequence of ```"ABCDE"``` while ```"AEC"``` is not).

Here is an example:
```S = "rabbbit", T = "rabbit"```

Return ```3```.



The discription craps. In plain English, it asks for how many ways do we have to generate T by only removing, but not reordering chars in S?

In the example, we can remove any one of the three ```'b'``` to generate "rabbit", so the answer is ```3```.


---

