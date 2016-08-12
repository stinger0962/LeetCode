# Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.




---



###The Algorithm

Starting from the 1st string, compare its characters one by one to that of each other string.

###The Code

```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        bool isDone = false; // If the comparison is done
        string prefix ="";
        if(strs.empty()){
            return prefix;
        }
        string str = strs[0]; // Compare chars of the 1st string to that of each other string
        for(int i = 0; i < str.size(); i++){ // Loop chars in the 1st string
            char ch = str[i];
            if(isDone){
                break;
            }
            for(int j = 0; j < strs.size(); j++){
                string strComp = strs[j];
                if(strComp.size() < i + 1 || strComp[i] != ch){
                    isDone = true;
                    break;
                }
            }
            if(!isDone){
                prefix.append(1, ch);
            }
        }
        return prefix;
    }
};
```