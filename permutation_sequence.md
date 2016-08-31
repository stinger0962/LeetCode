# Permutation Sequence


The set ```[1,2,3,…,n]``` contains a total of n! unique permutations.

By listing and labeling all of the permutations in order,
We get the following sequence (ie, for n = 3):

```
"123"
"132"
"213"
"231"
"312"
"321"```
Given n and k, return the kth permutation sequence.

Note: Given n will be between 1 and 9 inclusive.



---


###Not a Next Permutation

At first glance, we may attempt to solve the problem calling next_permutation k times. It looks fine. Yet, some test cases of which k is extremely large will cause an exceed of run time. So let's use a new approach.


###Take Advantage of Input Pattern

The input number has a very specific pattern. It's one of the followings:  ```1, 12, 123, ..., 123456789```. 

We also need to take use of some mathmatic knowledge that the number of permutations of N unique numbers is N! (N's factorial)

Using the above knowledge, input starting from 1 has (N-1)! permutations , same with input starting from 2, 3, ... ,n.

Now we see a recursion pattern hoving at the horizon. The rest is to carefully set up our recursion. Since n is a small number between 1 to 9, for our convience, we will use iterations.

###The Code

```
class Solution {
public:
    // Repeatedly call the function will result an exceeding in time limit 
    // So we use an alternative way, taking advantage of the known pattern of the input number
    string getPermutation(int n, int k) {
        
        // Store factorial of 0 to 8
        int fact[9];
        fact[0] = 1;
        for(int i = 1; i <9; i++){
            fact[i] = i * fact[i-1];
        }
        string result = "";
        // Put 1~n into an array
        vector<int> nums;
        for(int i = 1; i <=n; i++){
            nums.push_back(i);
        }
        // Decrement k by 1 because k is 1 based while our index is zero based
        k = k - 1;
        // Main loop, each loop will determine the left most digit 
        for(int i = n; i > 0; i--){
            int first = k / fact[i-1];
            k = k % fact[i-1];
            result.append(to_string(nums[first])); 
            nums.erase(nums.begin() + first);
        }
        return result;
    }


};
```



