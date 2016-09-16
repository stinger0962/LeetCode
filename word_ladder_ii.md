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


