# Numbers of Islands

A 2d grid map of m rows and n columns is initially filled with water. We may perform an addLand operation which turns the water at position (row, col) into a land. Given a list of positions to operate, count the number of islands after each addLand operation. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example:

Given m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]].
Initially, the 2d grid grid is filled with water. (Assume 0 represents water and 1 represents land).

```
0 0 0
0 0 0
0 0 0

Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land.```

```
1 0 0
0 0 0   Number of islands = 1
0 0 0
Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land.```

```
1 1 0
0 0 0   Number of islands = 1
0 0 0
Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land.```

```
1 1 0
0 0 1   Number of islands = 2
0 0 0
Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land.```

```
1 1 0
0 0 1   Number of islands = 3
0 1 0
We return the result as an array: [1, 1, 2, 3]```

Challenge:

Can you do it in time complexity ```O(k log mn)```, where k is the length of the positions?




---



###Union Find

The hard work is how to implement the union find data structure. As for the idea, it is straightforward.

Initially, each node is an isolated subset which is its own root.

Each time we add a land, it becomes the root of the subset it belongs.

Then we check its four adjacent nodes.

If a neighboring node is a land and is not connected to the current node, union them.

See summary for more info about union find and implementation.

###The Code

```
class Solution {
    int findRoot(vector<int>& unionTable, int id){ 
        // path compression during find, 
        // Approach 1: update value of all nodes alone the path to root 
        // if(unionTable[id] == id) return id;
        // unionTable[id] = findRoot(unionTable, unionTable[id]);
        // return unionTable[id];
        
        // Approach 2: Update each node's value to its grandparent
        while(unionTable[id] != id){
            unionTable[id] = unionTable[unionTable[id]];
            id = unionTable[id];
        }
        return id;
    }
public:
    vector<int> numIslands2(int m, int n, vector<pair<int, int>>& positions) {
        vector<int> result; // Return value
        int count= 0; // Record number of islands
        vector<int> unionTable(m*n, -1); // Initialize the union table to all -1
        vector<vector<int>> dirs = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}}; // Four directions
        for(auto pos: positions){
            // Initially, assign each new land as the root of its subset
            int land = n * pos.first + pos.second;
            unionTable[land] = land;
            // To count islands correctly, increment the count by 1 each time we build a land
            // Then check its four adjacent nodes, decrement count by 1 each time an adjacent node is a land 
            ++count; 
            // Check four directions
            for(auto dir: dirs){
                // Coordinates of the adjacent node
                int x = pos.first + dir[0];
                int y = pos.second + dir[1];
                int neighbor = x * n + y;
                // If neighbor is out of boundary or is water, move to next direction
                if(x < 0 || x >= m || y < 0 || y >= n || unionTable[neighbor] == -1) continue;
                // If root of current land and root of neighboring land are different, union both ROOTS
                // Notice that we are union roots of subsets, while not subsets themselves
                // We also conduct an optimization(pass compression) during the process of findRoot.
                int rootNb = findRoot(unionTable, neighbor);
                int root = findRoot(unionTable, land);
                if(rootNb != root){
                    unionTable[root] = rootNb;
                    --count;
                }
            }
            result.push_back(count);
        }
        return result;
    }
};
```

