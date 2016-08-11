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

Below is the pseudocode of backtracking with detailed explanation:

```
procedure DST(cell)
  if reject(cell) then return
  // (*)we cannot use valid candidate(cell) here
  if accept(cell) then output(matching word)
  cell ← visited
  adj ← adjacent(cell) // First extension
  while adj is within boundary do
    DST(adj)
  cell ← unvisited
  // (*)we cannot use invalid candidate(cell) here
```
1. The **reject** condition has two meanings, either a word does NOT exist, or a cell is already visited.

2. The **accept** condition means that we encounter a leaf node in the trie.

3. To avoid using an extra mxn space, we temporarily change a cell's char into **'#'** after it has been **visited**, and switch it back to its original char after a DST is done.

4. We CANNOT use **valid and invalid candidate** in this problem since **each cell is itself a root**. 
Valid/Invalid candidate is only appliable when **there is a single root**.

5. The above(4) causes a problem. We will not be able to track previous characters when composing a word, since we don't know where is the appropriate root. Thus, we need to modify our Trie node. Instead of storing a boolean value, we **store the word itself in a leaf node**. 

6. The rest task is to **carefully** construct the boundary conditions. As you will see, the structure of DFS is **similar** to that of a **maze generator** function.

###The Code

```
// We only need a TrieNode in this problem. No add, no search
// Besides a boolean value, we also store the word in the node
class TrieNode{
public:
    TrieNode* next[26];
    string word;
    bool isWord;
    TrieNode():isWord(false), word(""){
        memset(next, 0, sizeof(next)); // Optimization of initialization
    }
};


class Solution {
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        TrieNode* root = loadWords(words);
        vector<string> result;
        for(int i = 0; i < board.size(); i++){
            for(int j = 0; j < board[0].size(); j++){
                findWordsHelper(board, root, i, j, result);
            }
        }
        return result;
    }

private:
    // Build a trie loaded with all the words 
    TrieNode* loadWords(vector<string>& words){
        TrieNode* root = new TrieNode();
        for(string word: words){
            TrieNode* curr = root;
            for(char c : word){
                if(!curr->next[c-'a']){
                    curr->next[c-'a'] = new TrieNode();
                }
                curr = curr->next[c-'a'];
            }
            curr->word = word;
            curr->isWord = true;
        }
        return root;
    }
    // Backtracking function of the cell [i][j]
    // During the DFS, we temporary change the char in a visited cell to '#', and switch it back after DFS is done. 
    void findWordsHelper(vector<vector<char>>& board, TrieNode* root, int row, int col, vector<string>& result){
        // Ending condition is similar to a maze generator
        if(row < 0 || row >= board.size() || col < 0 || col >= board[0].size() || board[row][col]=='#'){ 
            return;
        }
        char c = board[row][col];
        root = root->next[c-'a'];
        if(root == nullptr){
            return;
        }
        if(root->isWord){// Accept : word found 
            result.push_back(root->word);
            root->isWord = false;
        }
        board[row][col] = '#'; // Set visited
        findWordsHelper(board, root, row-1, col, result);
        findWordsHelper(board, root, row, col-1, result);
        findWordsHelper(board, root, row+1, col, result);
        findWordsHelper(board, root, row, col+1, result);
        
        board[row][col] = c; // Set unvisited

    }
};
```






