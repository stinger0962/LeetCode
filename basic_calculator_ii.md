# Basic Calculator II

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, +, -, *, / operators and empty spaces . The integer division should truncate toward zero.

You may assume that the given expression is always valid.

Some examples:
```
"3+2*2" = 7
" 3/2 " = 1
" 3+5 / 2 " = 5
```
Note: Do not use the ```eval``` built-in library function.



---

###Similarity to Series I
Similar to the previous one, the real challenge is when we encounter an operator that changes the priority of the expression. 



We use two stacks, (one for number, one for operator) to record status when we enter an expression of high priority. (similar to previously opening parenthesis)

We pop value from stacks and do one more calculation when we get off from high priority expression. ( similar to previously closing parenthesis)


###New Inspiration

A smart solution is that we use one stack to store segmented result.

When we encounter a number, we record that number.

When we encounter an operator, before we record that op, we push a segmented result onto the stack.

However, when operator is ```*``` or ```/```,  before we push, we need to pop one number from the stack first.

The best practice to understand this algorithm is to run some examples on the paper.


###The Code

*My Solution*

```
class Solution {
    
// Do one step of calculation
// result: 1st operand, also the result of calculation
// op: operator
// number: 2nd operand
int cal(int result, char op, int number){
    switch(op){
        case('+'):
            result += number;
            break;
        case('-'):
            result -= number;
            break;
        case('*'):
            result *= number;
            break;
        case('/'):
            result /= number;
            break;
    }
    return result;
}

public:
    int calculate(string s) {
        stack<int> numStack;
        stack<char> opStack;
        int res = 0;
        int num = 0;
        char op = '+';
        for(int i = 0; i < s.size(); i++){
            // Read number and consecutive numbers
            if(s[i] -'0' >=0){
                num = num * 10 + s[i] - '0';
            }
            // Read * and /
            // If stack is empty, operators are in curve, push result and op onto stack, do no calculation
            // If stack is not empty, operators are in order, consume one op
            else if(s[i] == '*' || s[i] == '/'){
                if(numStack.empty()){
                    numStack.push(res);
                    opStack.push(op);
                    res = num;
                }
                else{
                    res = cal(res, op, num);
                }
                // Clear number, and update op
                num = 0;
                op = s[i];
            }
            // Read + and -
            // If stack is not empty, operators are in curve, consume one op, then pop num and op from stack, consume one more op
            // If stack is empty, operators are in orderm consume one op
            else if(s[i] == '+' || s[i] == '-'){
                if(numStack.empty()){
                    res = cal(res, op, num);
                }
                else{
                    res = cal(res, op, num); // Result of high priority expression
                    num = res; // Result is 2nd operand
                    res = numStack.top(); // Retrieve 1st operand from stack
                    numStack.pop();
                    op = opStack.top(); // Retrieve operator from stack
                    opStack.pop();
                    res = cal(res, op, num); // Consume op retrieved from stack, result is the final result so far
                }
                num = 0;
                op = s[i];
            }
            // Do nothing if a char is white space
        }
        // Don't forget the last number
        res = cal(res, op, num);
        // If stack is not empty, do one more calculation
        if(!numStack.empty()){
            num = res; // Result is 2nd operand
            res = numStack.top(); // Retrieve 1st operand from stack
            numStack.pop();
            op = opStack.top(); // Retrieve operator from stack
            opStack.pop();
            res = cal(res, op, num); // Consume op retrieved from stack, result is the final result so far
        }
        return res;
    }
};
```

*A Smart Solution*

