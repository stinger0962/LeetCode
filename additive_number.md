# Additive Number

Additive number is a string whose digits can form additive sequence.

A valid additive sequence should contain at least three numbers. Except for the first two numbers, each subsequent number in the sequence must be the sum of the preceding two.

For example:
```"112358"``` is an additive number because the digits can form an additive sequence: ```1, 1, 2, 3, 5, 8.```

```
1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
```

```"199100199"``` is also an additive number, the additive sequence is: ```1, 99, 100, 199.```
```
1 + 99 = 100, 99 + 100 = 199
```
Note: Numbers in the additive sequence cannot have leading zeros, so sequence 1, 2, 03 or 1, 02, 3 is invalid.

Given a string containing only digits ```'0'-'9'```, write a function to determine if it's an additive number.

Follow up:
How would you handle overflow for very large input integers?


---


###Walk through

  This is a problem of verifying a combination of three numbers. We will use a double loop to choose 1st and 2nd number, and use either iteration or recursion to verify the 3rd number.
  
  There are two parts in the problem.
  
  1. In the double outer loops, we loop through every possible combination of 1st and 2nd numbers. To reduce run time, we do add constraints of their length. 
  
  2. n the inner loop/recursion, we traverse the string, repeatedly verifying if its leading characters match the sum of 1st and 2nd number. 
  
  The **trick** here is that we don't compare numbers, but match strings.
  
###Get Rid of Integers

The answer of the follow-up question is to rid the integers by providing a string add function. We get the sum of first two numbers by calling the string add function, then verify if the lading chars match the string sum.

###Tail Recursion

A tail recursion is a recursion that the recursive call is the last statement in the function.

**Pro**: Tail function is important in that it doesn't use stack space. (because it merely call another recursion before it returns)

**Con**: difficult for human being to understand. We can always convert a tail recursion to a loop if it's hard to understand.

**How** to convert tail recursion to loop:  
  1. Compare the parameters in the function definition and the last recursive call. The difference is how we will move forward in a loop.
  2. The ending condition in the recursion is also that of a loop.

**When** to use tail recursion:

  This problem is a good example of when to use tail recursion. 
  
  A general scenario to use tail recursion is that when we want to determine T/F of a set, which is only true if a sequence of its subset is T. 
  
 ```
 set = true if (subset#1 && subset#2 && setset#3 && ... setset#k)
 Recursion==>
 bool isTrue(set begin@#1)
     return subset1 == true && isTrue(set begin@#2) 
 Loop==>
 bool isTrue(set begin@#1)
     for(N = 1 to k)
       if subset#N == false
         return false
     return true
 ```

###The code

```
class Solution {
public:
    // i and j are the length of the 1st and 2nd number
    // Double loop on i and j
    // Apply some constraints on i and j to reduce run time
    bool isAdditiveNumber(string num) {
        for(int i = 1; i <= num.size() / 2; ++i){
            if(i > 1 && num[0] == '0') return false; // Illegal leading number b/c of leading zero
            for(int j = 1; j <= num.size() - i - max(i, j); ++j){
                if(j > 1 && num[i] == '0') break; // Illegal 2nd number b/c of leading zero
                long x1 = stol(num.substr(0, i));
                long x2 = stol(num.substr(i, j));
                if(isValid(x1, x2, i+j, num)) return true; // Check the string for 
            }
        }
        return false;
    }
    
private:
    // Tail recursion: return true if the string is valid
    // x1: the 1st number
    // x2: the 2nd number
    // start: the starting index of searching range (the 3rd number)
    bool isValid(long x1, long x2, int start, string num){
        if(start == num.size()) return true;
        x2 = x2 + x1; // Assign x1+x2 to x2
        x1 = x2 - x1; // Assign x2 to x1
        string strX2 = to_string(x2);
        return num.substr(start).find(strX2) == 0 && isValid(x1, x2, start + strX2.size(), num);
    }
    
    // Loop version
    
};
```




