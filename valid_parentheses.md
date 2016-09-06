# Valid Parentheses


Given a string containing just the characters ```'(', ')', '{', '}', '[' and ']'```, determine if the input string is valid.

The brackets must close in the correct order, ```"()"``` and ```"()[]{}"``` are all valid but ```"(]"``` and ```"([)]"``` are not.





---



###Maintain a Stack 

We maintain a stack, first in first out, of all open parentheses.

When we encounter a closing parentheses, check the top of the stack.

If it's not the matching one, return false immediately. Otherwise, pop top from the stack.

When we are done, we should have an empty stack. Otherwise return false.


###The Code

```
class Solution {
public:
    bool isValid(string s) {
        vector<char> chars;
        // Loop through the string
        for(char ch: s){
            switch(ch){
                case '(':
                case '[':
                case '{':
                    chars.push_back(ch);
                    break;
                case ')':
                    if(chars.empty() || chars.back() != '(') return false;
                    chars.pop_back();
                    break;
                case ']':
                    if(chars.empty() || chars.back() != '[') return false;
                    chars.pop_back();
                    break;
                case '}':
                    if(chars.empty() || chars.back() != '{') return false;
                    chars.pop_back();
                    break;
            }
        }
        
        if(chars.empty()){
            return true;
        }
        else{
            return false;
        }
    }
};
```