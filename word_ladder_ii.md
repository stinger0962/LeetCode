# Word Ladder II


Given two words (beginWord and endWord), and a dictionary's word list, find all shortest transformation sequence(s) from beginWord to endWord, such that:

Only one letter can be changed at a time
Each intermediate word must exist in the word list
For example,

Given:
beginWord = ```"hit"```

endWord = ```"cog"```

wordList = ```["hot","dot","dog","lot","log"]```

Return
```
[
    ["hit","hot","dot","dog","cog"],
    ["hit","hot","lot","log","cog"]
  ]```
Note:
All words have the same length.

All words contain only lowercase alphabetic characters.



---

###Combination of BFS and DFS

This is a comprehensive problem involved in both BFS(finding shortest path), and DFS(finding all solutions).

For more details about BFS, please refer to *14.1 Word Ladder I*.


We will discuss how we construct BFS and DFS, and how we combine these two algorithm into one.

**In summary, we set up an adjacent graph and define valid moves in BFS, apply backtracking on the graph in DFS.**

###Breadth First Search

First, let's have a look at BFS. 

1. We still use a queue to do traversal in BFS.
2. Maintain a ```map<string, vector<string>>``` to represent an **adjacent graph**, which stores pairs of word and all of its valid next words.
3. Maintain a ```map<string, int>``` to represent a word and the shortest steps taken to transform from start word. This map will be used to **validate** if **moves** from word to word satisfy shortest path requirement.

**3 is key**, and 3 stands true because of the following statement.

Each word, if they are able to be transformed, has its own shortest path. 

If a word is on a shortest path, then steps to this word must equal to the shortest path of the word.

Let's see an example. 

Say we two path. 

A. ```start->->-> mid->->->end```.  (6 steps)

B. ```start->->->->mid->end```. (5 steps)

At first glance, solution B has is shorter than A although it takes more steps to reach mid than that of A.

But unfortunately, neiher is the shortest solution. A shortest solution is as below:

C. ```start->->->mid->end``` 

In the shortest path from start to end, steps to mid(3) equals to the shortest path from start to mid(3).

###Depth First Search

Now, let's have a look at DFS. 

In BFS, we have set up an adjacent graph (```map<string, vector<string>>```) and a collection of valid moves (```map<string, int>```). In DFS, we will traversal the graph, and find all shortest paths.

```
DFSRecur(string curr)
For the current word curr, loop through its next words 
  If shortest path of a next word == shortest path of curr + 1, then the next word is a candidate
    Push candidate to the solution
    DFSRecur(candidate)
    Remove candidate from solution ```

We can see that the structure in DFS is that of a typical backtracking algorithm.

The hard core of this problem is how to construct an adjacent graph and define valid moves in BFS.

###The Code

```

```
