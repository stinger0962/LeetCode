# Longest Consecutive Sequence

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

For example,

Given ```[100, 4, 200, 1, 3, 2]```,

The longest consecutive elements sequence is ```[1, 2, 3, 4]```. Return its length: 4.

Your algorithm should run in O(n) complexity.



---



###Speaking of Time Complexity O(N)


We can figure out what data structure to use by the requirement of time complexity.

Since we have to run through the array at least once, which takes o(N), time complexity of each number is thus limited to O(1). 

The conclusion naturally leads us to thinking about using a **hashtable**.

###The Algorithm (a Variation of Union Find)


I've put a summary of union find in the OneNote. We will not cover it here.

For each **key** in the array, we keep a **value** represents the longest length of sequence it belongs to.

For each number n in the array, we do the following things.

```
1. Retrieve H[n+1], H[n-1], which are n's adjacent numbers.
2. Insert n's value as the sum of H[n+1], H[n-1], and 1.
3. Update the hash value of the boundaries of the sequence n belongs to.
```

**Note** that hash value of numbers in the middle of a sequence will lose track during the process. We only keep track of two boundaries. This allows a o(1) time complexity on each number.


```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        // A hashtable storing numbers and their longest sequence
        unordered_map<int, int> myHash;
        int maxLen = 0; // Record the max length of a sequence
        // All actions in the loop takes O(1), so the total takes O(n) 
        for(int n : nums){
            if(myHash.count(n) == 0){
                // Retrieve value for both neighbors
                int left = myHash.count(n-1) ? myHash[n-1] : 0;
                int right = myHash.count(n+1) ? myHash[n+1] : 0;
                int sum = left + right + 1;
                // Insert new value
                myHash[n] = sum;
                maxLen = max(maxLen, sum);
                
                // Update both boundaries of the sequence
                // If left/right = 0, nothing is updated
                myHash[n-left] = sum;
                myHash[n+right] = sum;
            }
            // Skip duplicate numbers
            else{
                continue;
            }
        }
        return maxLen;
    }
};
```

