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




###Setting up Ending Condition



###Variation of Backtracking Structure


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
