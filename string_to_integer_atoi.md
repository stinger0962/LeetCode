# String to Integer (atoi)

Implement atoi to convert a string to an integer.

Hint: Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

Notes: It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.




---



**The fun of this problem is requirement to deal with all invalid inputs.**

As the spoilers show: 


Requirements for atoi:

1. The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

2. The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

3. If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

4. If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, INT_MAX (2147483647) or INT_MIN (-2147483648) is returned.



---

## Convert char '0' to int 0

The technique is about how to convert ```char a = '7'``` to a  ```int 7``` ?

We would need the knowledge that implicitly converting a char to int returns its ACSII code.
Thus we will need to **remove the offset of '0'.**

```
char a = '7';
int i_a = a - '0'; // i_a = 7
```




Code is thus straightforward. By the way, the accepted rate is just a little above 10%, notably low for an easy problem. 



---



```
class Solution {
public:
    int myAtoi(string str) 
    {
        // Returning value
        long res = 0;
        if(str.length() == 0)
        {
            return 0;
        }
        // Trim white spaces from the beginning
        size_t first = str.find_first_not_of(' ');
        str = str.substr(first);
        if(str.length() == 0)
        {
            return 0;
        }
        // A signal of negative sign
        bool isNeg = str[0] == '-' ? true : false;
        // Deal with possible +/- signs
        if(str[0] == '+' || str[0] == '-')
        {
            str = str.substr(1);
        }
        // Loop the string and construct the integer
        for(int i = 0; i < str.length(); i++)
        {
            if(!isInt(str[i]))
            {
                break;
            }
            // Needs to remove the offset '0' to realign it to the count from 0~9
            int dig = str[i] - '0';
            res = res * 10 + dig;
            // Stop and return if the int goes beyond the bound
            if(res > INT_MAX)
            {
                return isNeg ? INT_MIN : INT_MAX;
            }
        }
        
        return isNeg ? 0 - res : res;
    }
private:
    // A helper function determines if a character is an integer
    // Convert a char to int implicitly return its ACSII code, 
    // Needs to remove the offset '0' to realign it to the count from 0~9
    bool isInt(char c)
    {
        int ic = c-'0';
        if(ic < 0 || ic > 9)
        {
            return false;
        }
        else
        {
            return true;
        }
    }
};
```