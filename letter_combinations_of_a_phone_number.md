# Letter Combinations of a Phone Number

Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.



Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
Note:
Although the above answer is in lexicographical order, your answer could be in any order you want.

**My Note**: for 0 to 9, characters each number may represent are as below:


```
{"0","1","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
```

###Build Result on the Fly

The basic idea is that each time we add a number, the result set expands from m to ```m*n```, where n is the number of chars the new number can represent.

Thus we build the string set on fly. Each time we enter the loop, we build a temporary string set, add ```m*n``` strings to it. At the end of each loop, we replace the result by the temporary string set.





###The Code

```
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> result;
        if(digits == ""){
            return result;
        }
        // Must initialize and add one string into result, 
        result.push_back("");
        string charmap[10] = {"0","1","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
        for(int i = 0; i < digits.size(); i++){
            // Create a temporary vector at the enter of each loop, 
            // Push result.size() * newchars.size() strings into temp (this is also the number of new possibilities) 
            // Result will be replaced by this temp at the end of each loop
            vector<string> temp;
            // digits[i] is the ASCII code of a char while implicitly converted to int
            string chars = charmap[digits[i] - '0'];
            for(int c = 0; c < chars.size(); c++){
                for(int j = 0; j < result.size(); j++){
                    temp.push_back(result[j] + chars[c]);
                }
            }
            result = temp;
        }
        return result;
    }
};
```