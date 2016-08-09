# Integer to Roman


Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.


---


###Roman Numerals

| 1 | 5 | 10 | 50 | 100 | 500 | 1000 |
| -- | -- | -- | -- | -- | -- | -- |
| I | V | X | L | C | D | M |




###Let's Cast a Magic
For a discrete problem like this, we can list out all the possibilities.


###The Code

```
class Solution {
public:
    string intToRoman(int num) {
        string thu[4] = {"", "M", "MM", "MMM"};
        string hun[10] = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
        string ten[10] = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
        string one[10] = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
        
        return thu[num / 1000] + hun[(num / 100) % 10] + ten[(num / 10) % 10] + one[num % 10];
    }
};
```
