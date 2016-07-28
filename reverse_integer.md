# Reverse Integer

Reverse digits of an integer.

Example1: x = 123, return 321
Example2: x = -123, return -321




---

We want to solve the problem with a single run from the beginning to the end so that the run time is O(n).

The algorithm requires building the reversing integer on the fly.

```
class Solution {
public:
    int reverse(int x) 
    {
        
        bool isNeg = x > 0 ? false : true;
        x = abs(x);
        long rev = 0;
        while(x > 0)
        {
            rev = rev * 10 + x % 10;
            x = x / 10;
        }

        if(rev>INT_MAX)
        {
            return 0;
        }
        return isNeg ? 0-rev : rev; 
    }
};```
