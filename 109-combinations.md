Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

For example,
If n = 4 and k = 2, a solution is:




###Solution

Typical backtracking.

Structure as below:


```
BT(para list, int index) 
    for (j = index ~ max)
        add(j)
        BT(para list, j+1)
        delete(j)

```


```
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> result;
        vector<int> path;
        combineBT(result, path, n, k, 1);
        return result;
    }
    
private:
    void combineBT(vector<vector<int>>& result, vector<int>& path, int n, int k, int i) {
        // if (i > n) return;
        if (path.size() == k) {
            result.push_back(path);
            return;
        }
        for (int j = i; j <= n; j++) {
            path.push_back(j);
            combineBT(result, path, n, k, j+1);
            path.pop_back();
        }
        
    }
};

```

