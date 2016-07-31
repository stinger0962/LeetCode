# Course Schedule

There are a total of n courses you have to take, labeled from 0 to n - 1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: ```[0,1]```

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

For example:

```2, [[1,0]]```

There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.

```2, [[1,0],[0,1]]```

There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

**Note**:
The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.



**Hints**:
This problem is equivalent to **determining if a cycle exists in a directed graph**. 

**If a cycle exists**, no topological ordering exists and therefore it will be **impossible** to take all courses.



---


First, let's set up some basic knowledges.

**Edge List**: (How the graph is represented)

https://www.khanacademy.org/computing/computer-science/algorithms/graph-representation/a/representing-graphs


**Topological sorting**: (Algorithm to find a topological sorting)

https://en.wikipedia.org/wiki/Topological_sorting#Depth-first_search



---


An algorithm based on the wiki's depth first traversal (DFT) is as below: 

```
Loop through all vertices in the graph
  if DFTrecur(vertex) == false
  return false
return true

DFTrecur(vertex n)
  if n's status = permenant mark(visited)
    return true
  if n's status = temporary mark(under DFT)
    return false
  for each m that an edge exists n->m
    if(DFTrecur(m) == false)
      return false
  mark n permenant
  return true
```

**
## The structure of a recursive function returning boolean **


In a scenario that one existence of certain condition means false(or true, it doesn't matter)   

When a recursive call **depth@ N returns false**, all the shallow calls, **depth@ N-1 to 0, should also return false**. 
```
bool Recur(parameter list)
  // Ending condition
  if ...
    return F
  if ...
    return T
  // Recursive call
  if (Recur(...) == F ) 
    return F
  return T
  
```




---


```
class Solution {
public:
    // In this problem, a pair of [a, b] represents an edge b-->a
    //                             ^  ^
    //                             in out
    // Same as : b has an outgoing edge to a; a has an incoming edge from b
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) 
    {
        // Create the graph of vertices and their outgoing edges
        // Outgoing edges are simplified to an int representing its destination
        vector<vector<int>> graph(numCourses);
        // a node has three status
        // 'N' : initial status which means the node's DFT hasn't started yet
        // 'P' : a permenant mark which means the node's DFT is done and no cycle found
        // 'T' : a temporary mark which means the node is part of the processing DFT
        // More explanation: 
        // A cycle exists if a DFT encounters a node marked with 'T'
        // Such a cycle starts and ends with the marked node
        vector<char> status(numCourses, 'N');

        // Fill the graph with data
        for(auto pre: prerequisites)
        {
            graph[pre.second].push_back(pre.first);
        }
        
        // Call DFT for all the nodes 
        // Terminate the loop if a cycle is found in the process, the course can't be finished
        for(int i = 0; i < numCourses; i++)
        {
            if(!topoDFTrecur(i, graph, status) )
            {
                return false;
            }
            
        }
        return true;
    }
    
private:
    bool topoDFTrecur(int vertex, vector<vector<int>>& graph, vector<char>& status)
    {
        // 'T' means a cycle exists, and we are done
        if(status[vertex] == 'T')
        {
            return false;
        }
        // 'P' means DFT of this node is already done, go to next node
        if(status[vertex] == 'P')
        {
            return true;
        }
    
        // Begin DFT recursively
        status[vertex] = 'T';
        // For each destination of an edge outgoing from vertex, recursively call its DFT
        vector<int> dests = graph[vertex];
        for(auto dest: dests)
        {
            if(!topoDFTrecur(dest, graph, status) )
            {
                return false;
            }
        }
        status[vertex] = 'P';
        return true;
    }
};
```




