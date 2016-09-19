# Basic Calculator


Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ```(``` and closing parentheses ```)```, the plus ```+``` or minus sign ```-```, non-negative integers and empty spaces .

You may assume that the given expression is always valid.

Some examples:
```
"1 + 1" = 2
" 2-1 + 2 " = 3
"(1+(4+5+2)-3)+(6+8)" = 23
```
Note: Do not use the eval built-in library function.



---



###The General Process

First, we need a guideline about the general process.

Generally, we will keep **three variables**,  **result**,  operator, and operand.

When we encounter an **operator**, we record it. In this problem, it is a sign flip between 1 and -1.

When we encounter an **operand**, we **consume** an operator and the operand itself to generate new result.

Each operator pairs with an operand, and operator is **preceding** to its pairing operand.

To make this work, we need to assign an operator + before the first number.

Let's see an example.

```5 + 4```

The initial variables: ```result = 0, sign = 1, operand = 0```

First, we read 5, consume a sign, and generate new result: 

```operand = 5, result = result + sign * operand = 0 + 1 * 5 = 5```

Next we read +: ```sign = 1```

Finally, we read 4. Again, we consume a sign and generate new result:
```operand = 4, result = result + sign * operand = 5 + 1 * 4 = 9```

###Tackle with Parentheses

The above algorithm works smoothly until we encounter an open parenthesis.

To understand how parenthese work, we can think of stuff inside a pair of parentheses a **new expression with high priority**. 

When we enter a parenthesis, we will calculate the expression inside first. We won't resume to the general process until expression between () becomes a single result.

Therefore, we need a **stack** to store previous operand and operator when we are inside a parenthesis.

Let's look at an example.

```5 - (4 + 2)```

When we are about to enter the parenthesis, we have ```result = 5, sign = -1```.

We will **push** those onto a stack, and **reset** their values because we are going to deal with a new expression inside the parenthesis. 

When we are about to quit the parenthesis, we have ```result = 6```, the final result of expressions between parentheses.

We can think of this result as operand2, while we have stored operand1 and operator on the stack. Pop them from stack in proper order and calculate new result as ```operator(operand1, operand2)```


###The Code

```
class Solution {
public:
    int calculate(string s) {
        int len = s.size();
        int operand = 0; // Current operand, each operand consumes a sign
        int sign = 1; // Current sign(operator), each +/- alters sign's value(1/-1)
        int result = 0;
        stack<int> storage; // The stack used to store result and sign when entering a parenthesis
        for(int i = 0; i < len; i++){
            // Read integers, including possible consecutive integers
            if(s[i] >= '0' && s[i] <= '9'){
                operand = operand * 10 + s[i] - '0';
                if(i < len - 1 && s[i+1] < '0'){ // If next char is not integer, consume a sign, and reset operand
                    result += operand * sign;
                    operand = 0;
                }
            }
            // Read sign
            else if(s[i] == '+' || s[i] == '-'){
                sign = s[i] == '+' ? 1 : -1;
                // operand = 0;
            }
            // Open parenthesis means a new start.
            // Push current result and sign onto stack, then reset their values.
            // Stored value will be called when parenthesis ends.
            else if(s[i] == '('){
                storage.push(result);
                storage.push(sign);
                result = 0;
                sign = 1;
            }
            // When encountering close parenthesis, result is the final answer in the ()
            // We can consider result as operand2, while operand1 and sign are stored on the stack
            // Retrieve op1 and sign from the stack and get the new result
            else if(s[i] == ')'){
                sign = storage.top();
                storage.pop();
                operand = storage.top(); // op1
                storage.pop();
                result = operand + sign * result; // result is op2, new result = op1 + sign * op2
                operand = 0;
            }
        }
        // Don't forget the last operand
        if(operand){
            result += operand * sign;
        }
        return result;
    }
};
```

