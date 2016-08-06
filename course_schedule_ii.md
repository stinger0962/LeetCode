# Course Schedule II



There are a total of n courses you have to take, labeled from ```0``` to ```n - 1```.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: ```[0,1]```

Given the total number of courses and a list of prerequisite **pairs**, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

For example:

```2, [[1,0]]```

There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is ```[0,1]```

Another example:

```4, [[1,0],[2,0],[3,1],[3,2]]```

There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. So one correct course order is ```[0,1,2,3]```. Another correct ordering is```[0,2,1,3]```.



---


###Topological Sorting

This problem is equal to find out a topological sorting for the course list. Refer to *8.1 Course Schedule* for more details.



### The DFT O(V+E) Algorithm 
```
L ← Empty list that will contain the sorted nodes
while there are unmarked nodes do
    select an unmarked node n
    visit(n) ```
```  
function visit(node n)
    if n has a temporary mark then stop (not a DAG)
    if n is not marked (i.e. has not been visited yet) then
        mark n temporarily
        for each node m with an edge from n to m do
            visit(m)
        mark n permanently
        unmark n temporarily
        add n to head of L```
        
One place we need to pay attention to is **when and how to add a vertex** to the sorted collection.

Each vertex is added to the **head** of the collection **at the end of its DFT**, providing no cycle found in the process. 


### The Code

```
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) {
        // Index of the outer vecter represents vertices, 
        // Destinations of outgoing edges are stored in the inner vector
        vector<vector<int>> graph(numCourses); 
        // Each vertex has a mark, which identifies if a cycle exists in the graph
        // Default: 0
        // Temporary mark (in traversal): -1
        // Permenant (DFT done): 1
        vector<int> marks(numCourses, 0);
        vector<int> sorted; // The returning vector
        // Add data into the graph
        for(auto pair: prerequisites){
            graph[pair.second].push_back(pair.first);
        }
        // Topological sorting, if cycle is detected, return an empty vector; otherwise, return sorted vertices
        for(int i = 0; i < numCourses; ++i){
            if(!topoSorting(i, graph, marks, sorted)){
                return vector<int>(0);
            }
        }
        // Correct order topological sort requires to add each vertex to the beginning of the collection
        // We will reverse the order of output to achieve the correct order
        reverse(sorted.begin(), sorted.end());
        return sorted;
    }
    
private:
    bool topoSorting(int ver, vector<vector<int>>& graph, vector<int>& marks, vector<int>& sorted){
        if(marks[ver] == -1){
            return false;
        }
        if(marks[ver] == 1){
            return true;
        }
        marks[ver] = -1;
        
        vector<int> destinations = graph[ver];
        for(auto des : destinations){
            if(!topoSorting(des, graph, marks, sorted)){
                return false;
            }
        }
        marks[ver] = 1;
        sorted.push_back(ver);
        return true;
    }
};
```