# Roman to Integer
Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.



---
###Observation

After a careful observation, we come to the conclusion that all Roman numerals have the following features:

1. Excluding subtraction (like IV), the numbers are listed in an descending order.
2. Also excluding subtraction, the number of a Roman is the sum of the number of each char.
2. In subtraction, the left char is less than the right one, and its value is [R - 2L]

The job is almost done as soon as these rules are set up.

###The Code

```
class Solution {
public:
    int romanToInt(string s) {
        int roman[26]; // An array of which index is Roman, value is integer
        roman['I'-'A'] = 1;
        roman['V'-'A'] = 5;
        roman['X'-'A'] = 10;
        roman['L'-'A'] = 50;
        roman['C'-'A'] = 100;
        roman['D'-'A'] = 500;
        roman['M'-'A'] = 1000;
        int result = roman[s[0] - 'A'];
        int prev = roman[s[0] - 'A']; // a pointer pointing to the value of previous char
        for(int i = 1; i < s.size(); i++){ // Loop through the string, adding the numbers
            result += roman[s[i] - 'A'];
            if(prev < roman[s[i] - 'A']){ // Do subtraction when properly
                result -= 2 * prev;
            }
            prev = roman[s[i] - 'A'];
        }
        return result;
    }
};
```
