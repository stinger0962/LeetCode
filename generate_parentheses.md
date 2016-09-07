# Generate Parentheses

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]```



---


###Come up to Backtracking

When we read "generate all combinations", we know it's time to call backtracking.



Recall that in building up solution using nodes/chars/array nums, the** basic structure** is:

```
add the candidate
call the function recursively
remove the candidate
```

As for **base case**, we can use 2 parameters which store the number of remaining left and right parentheses to be added. 

These two parameters have following restrictions:

```
0 <= Remaining left must <= remaining right


When both left and right = 0, finish recursion.
```

###The Code

Refer to Subset and combination sum for more details about backtracking of this structure.

```
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> result;
        string str = "";
        generateBacktrack(result, str, n, n);
        return result;
    }
private:
    // str: the string we are building
    // left: number of remaining left parentheses
    // right: number of remaining right parentheses
    void generateBacktrack(vector<string>& result, string& str, int left, int right){
        // Base case:
        // Invalid condition: there are more right parentheses than left, or too many left
        if(left > right || left < 0){
            return;
        }
        // Valid string found
        if(left == 0 && right == 0){
            result.push_back(str);
            return;
        }
        //Backtracking:
        // Be aware to 
        str.push_back('(');
        generateBacktrack(result, str, left-1, right);
        str.pop_back();
        
        str.push_back(')');
        generateBacktrack(result, str, left, right-1);
        str.pop_back();
    }
};
```