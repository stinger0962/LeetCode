# 7.3 Palindrome Number


Determine whether an integer is a palindrome. Do this without extra space.

---

The problem is actually ask for reversing an integer, then compare its value to the original one.

```
class Solution {
public:
    bool isPalindrome(int x) 
    {
        int temp = x;
        if(x < 0)
        {
            return false;
        }
        int rev = 0;
        while(temp > 0)
        {
            rev = rev * 10 + temp % 10;
            temp = temp / 10;
        }
        return rev == x;
    }
};
```