# Happy Number


```
Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: 
Starting with any positive integer, 
replace the number by the sum of the squares of its digits, 
and repeat the process until the number equals 1 
(where it will stay), 
or it loops endlessly in a cycle which does not include 1. 

Those numbers for which this process ends in 1 are happy numbers.

Example: 19 is a happy number

12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1```





---


This problem doesn't give any data structure explicitly, so the first issue is to **choose a proper data structure**.

There are two cases for each number -- being happy, or unhappy. Each case asks to find a loop, which **requires frequent look ups and matches**.  Hence, a **hashtable/hashset** well fit this requirement.

After making the right decision about what data structure to use, the rest part of the problem is quite straightforward.

*My runtime is 9ms, while most implementations involving math finish at 4 ms. The tradeoff between an easy-to-understand code and 5ms runtime is worth to me, at least at this moment*



---

```
class Solution 
{
public:
    bool isHappy(int n) 
    {
        map<int, int> happySet;
        int key = sumSquareDigit(n);
        happySet[n] = key; // the key to search for in the hashtable, it is the value of last entry
        while(happySet.find(key) == happySet.end())
        {
            happySet[key] = sumSquareDigit(key);
            key = sumSquareDigit(key);
        }
        if(key == 1)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
private:
    int sumSquareDigit(int number)
    {
        int digit = 0;
        int sum = 0;
        while(number != 0)
        {
            digit = number % 10;
            sum = sum + digit * digit;
            number = number / 10;
        }
        return sum;
    }
};```