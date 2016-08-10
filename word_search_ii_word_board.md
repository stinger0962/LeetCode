# Word Search II (Word Board)

Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

For example,
Given **words** = ```["oath","pea","eat","rain"]``` and **board** =

```
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]```
Return ```["eat","oath"]```.
Note:
You may assume that all inputs are consist of lowercase letters ```a-z```.



---
###First Thing First. Search Word? Use Trie


In this problem, however, we don't need to implement the entire class of a trie. 

**All we need is to define a Trie node.**

We don't need to implement ```add(string word)``` , or ```search(string word)``` since our add and search will be based on a word list and a 2D board, respectively.


### Find All the Solutions? Call Backtracking

This is a typical finding-all-the-solutions problem. We use backtracking to solve such a problem.

We need to determine several conditions in the following pseudocode of backtracking:

```
procedure DST(cell)
  if reject(P,cell) then return
  if accept(P,cell) then output(P,c)
  s ← first(P,c)
  while s ≠ Λ do
    DST(s)
    s ← next(P,s)
```
Here, the reject condition has two meanings, either a word does NOT exist, or a cell is already visited.

To avoid using an extra mxn space, we temporarily change a cell's char into '#' after it has been visited, and switch it back to its original char after a DST is done.




