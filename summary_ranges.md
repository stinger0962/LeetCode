# Summary Ranges

Given a sorted integer array without duplicates, return the summary of its ranges.

For example, given ```[0,1,2,4,5,7]```, return ```["0->2","4->5","7"]```.



---


###Intuitive Solution

Here is an intuitive solution.

First, we sort the array. Then we sweep the numbers, taking appropriate actions.


We use a flag to record if we are sweeping a consecutive sequence. 

If ```a[i] = a[i-1] + 1```, we set the flag on, and do no actions.

Else if flag is on, we update a range consisting of more than one numbers, ```"a->b"```

Else we update a range with a single number.

Don't forget to do one more step for the last number.

###The Code

```
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        vector<string> result;
        int n = nums.size();
        if(n == 0) return result;
        
        sort(nums.begin(), nums.end());
        string str = to_string(nums[0]);
        bool isCons = false; // Flag is on when sweeping consecutive integers
        for(int i = 1; i < n; i++){
            if(nums[i] == nums[i-1] + 1){ // Consecutive integers
                isCons = true;
            }
            else if(isCons){ // Update a range consisting of more than 1 number
                str.append("->").append(to_string(nums[i-1]));
                result.push_back(str);
                str = to_string(nums[i]);
                isCons = false;
            }
            else{ // Update a range with only a single number
                str = to_string(nums[i-1]);
                result.push_back(str);
                str = to_string(nums[i]);
                isCons = false;
            }
        }
        // Last number
        if(isCons){
            str.append("->").append(to_string(nums[n-1]));
            result.push_back(str);
        }
        else{
            result.push_back(str);
        }
        
        return result;
    }
};
```


