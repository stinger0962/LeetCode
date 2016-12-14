Given two numbers represented as strings, return multiplication of the numbers as a string.

**Note:**  


* The numbers can be arbitrarily large and are non-negative.
* Converting the input string to integer is **NOT **allowed.
* You should **NOT **use internal library such as **BigInteger**
  .
* ---



This is a pure math problem. The core to solve this problem is to understand how we do multiplication. 

We do multiplication digits by digits.



```
class Solution {
public:
    string multiply(string num1, string num2) {
        int m1 = num1.size();
        int m2 = num2.size();
        // multiply of two numbers of m1, m2 digits is at most m1+m2 digits
        vector<int> result(m1 + m2, 0);
        // go through from right to left so that result[p1] is initially zero in the inner loop
        for (int i = m1 - 1; i >= 0; i--) { // loop through 1st string, taking num1[i]
            for (int j = m2 - 1; j >= 0; j--) { // loop through 2nd string , taking num2[j]
                int mul = (num1[i] - '0') * (num2[j] - '0'); // the multiply of two corresponding digits
                // p1 and p2 are the index in the result where the mul of two digits will fit into
                int p1 = i + j;
                int p2 = i + j + 1;
                // now add mul to old value at result[p2], it consists of the two most-left digits of the result
                int sum = result[p2] + mul;
                // set new value for those two digits: result[p1] and result[p2]
                result[p1] = result[p1] + sum / 10;
                result[p2] = sum % 10;
            }
        }
        string str;
        for (auto i: result) {
            if (str.size() != 0 || i != 0) // ignore the leading 0
                str += to_string(i);
        }
        return str.size() == 0 ? "0" : str;
    }
};
```



![](https://drscdn.500px.org/photo/130178585/m%3D2048/300d71f784f679d5e70fadda8ad7d68f)

