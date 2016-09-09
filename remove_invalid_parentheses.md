# Remove Invalid Parentheses

Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

Note: The input string may contain letters other than the parentheses ```(```and ```)```.


Examples:

```
"()())()" -> ["()()()", "(())()"]
"(a)())()" -> ["(a)()()", "(a())()"]
")(" -> [""]
```

###Finding All Solutions, Use Backtracking

When we read about "all possible results", we know this is another backtracking problem.

We can think of removing invalid parentheses as constructing a new string from the given one, while skipping unneeded parentheses.

However, the ending condition and the body structure of backtracking is a little different, which makes this problem marked as hard.


###Setting up Ending Condition
Since we are to move minimum invalid parentheses, we are only to move either left, or right parenthesis. We will NOT remove both left and right in any condition. 

If we traversal the string while pairing valid parentheses, we will find out which one, and the number to remove. This counts for **the first ending condition**.



---



Furthermore, we want to avoid invalid sequences of parentheses. For example, ```"())" ```

To add this validation, we will introduce a new parameter "open". 

When we encounter a left parenthsis, "open" increments by 1; when we encounter a right one, it decrement by 1.

In a valid string, "open" is either positive or zero. 

When "open" is negative, we know there are too many ```')'```. This makes **the 2nd ending condition**.


###Variation of Backtracking Structure

Recall that a general body of backtrack includes 3 steps:

```
add this candidate to solution
Recur(next candidate) 
remove this candidate from solution
```

In previous problems, it looks like:

```
path.push_back(element)
recur(next element, new para list)
path.pop_back()
```

In this problem, we are going to skip a candidate, while not adding it. Thus, the structure looks a little different.

```
if(it is poosible to skip)
    recur(next char, skip this parenthesis)
recur(next char, add this parenthesis)
```
The logic is same as regular backtrack. We need to **undo a candidate after calling recursion on next candidate**. 

###The Code

```
class Solution {
public:
    vector<string> removeInvalidParentheses(string s) {
        // Left and right are the number of invalid parentheses to be removed
        int left = 0;
        int right = 0;
        for(char ch: s){
            if(ch == '(') left++;
            else if(ch == ')'){
                if(left > 0) left--;
                else right++;
            }
        }
        
        unordered_set<string> result;
        dpsHelper(result, s, "", 0, left, right, 0);
        return vector<string>(result.begin(), result.end());
    }
    
private:
    // result: the solution set
    // s: the string to be manipulated
    // path: the string we are building
    // index: char index in the traversal
    // left, right: the number of left/right parentheses to be removed
    // open: the number of opening parentheses waiting to be paired. These are considered valid. 
    void dpsHelper(unordered_set<string>& result, const string& s, string path, int index, int left, int right, int open){
        // Base case:
        if(index >= s.size()){
            if(left == 0 && right == 0 && open == 0) result.insert(path);
            return;
        }
        // This is why we need to keep this parameter. 
        // A negative open means that there are too many ')' 
        // e.g. Adding ')' to "()" will make an invalid string "())", and open is -1
        if(open < 0) return;

        // If char is not a parenthesis: add char to the path and move forward
        if(s[index] != '(' && s[index] != ')'){
            dpsHelper(result, s, path+s[index], index+1, left, right, open);
        }
        else{
            // If char is a left parenthesis
            if(s[index] == '('){
                // Next two lines is a viaration of typical three-line backtracking structure:
                
                // ******path.push_back(candidate);
                //       call recursive function
                //       path.pop_back()******
                
                // When encountering a left parenthesis, 
                // we first consider it invalid, if possible, generate according subsolution
                // Then considering it valid, generate the other subsolution 
                
                if(left > 0) dpsHelper(result, s, path, index+1, left-1, right, open);
                // open increments by 1 since there is one more opening parenthesis now
                dpsHelper(result, s, path+s[index], index+1, left, right, open+1);
            }
            // If char is a right parenthesis
            if(s[index] == ')'){
                if(right > 0) dpsHelper(result, s, path, index+1, left, right-1, open);
                dpsHelper(result, s, path+s[index], index+1, left, right, open-1);
            }
        }
    }
};
```
