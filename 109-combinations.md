
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

