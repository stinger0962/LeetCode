# Word Ladder I



---

Word Ladder II is a combination of BFS finding shortest path and DFS backtracking.

For more informations, refer to *10.8 Word Ladder II*



---



Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:

Only one letter can be changed at a time
Each intermediate word must exist in the word list
For example,

Given:
beginWord = ```"hit"```

endWord = ```"cog"```

wordList = ```["hot","dot","dog","lot","log"]```

As one shortest transformation is ```"hit" -> "hot" -> "dot" -> "dog" -> "cog"```,

return its length ```5```.

Note:
Return 0 if there is no such transformation sequence.

All words have the same length.

All words contain only lowercase alphabetic characters.



---


###Why Breadth First Search

If we apply other algorithms, such as DFS, we will make extra efforts to maintain the status of shortest path. 

While the **features of BFS ensures** that the path we find will be the shortest one. We will see why this is true in more details.

###Apply BFS on a Mimic Tree

In this problem, there is no data structure to apply BFS on, but we can make one that **mimics** the behaviors of a **tree**.

Let the begin word be the root of the tree.

For each node(word) in the tree, we **define its next nodes** as all the words in the word list that can be transformed from it by only changing one letter.

In the above example, begin word is ```'hit'```, and its next node is 'hot'.

While ```'lot'``` has three next nodes : ```'hot', 'dot', 'log'```.

Starting from the root(begin word), BFS always finds all next nodes for current level before it moves to next level. 

Thus,** using BFS, we will guarantee that the path we find is the shortest one.**

To further verify the statement, let's see an example.

(e.g.) There are two routes: ```1->2->3->4```  and ```1->3->4```

Starting from ```1```, which is at level 0, we want to find a shortest route to ```4```

BFS will put both ```2``` and ```3``` at level 1 b/c they are both valid next nodes to ```1```.

Thus 4 is placed in level 2, and a shortest path is guaranteed. 

###The Code

```
class Solution {
public:
    // Breadth First Search fits the requirement since we are looking for a SHORTEST route
    // The basic idea of BFS: (similar to BFS of tree)
    
    // For words in each level of queue, 
    // 1. search for their valid next words in the list
    // 2. add them into BST queue
    // 3. remove them from the list
    // **A valid next word** is a word in the word list that can be transformed from input word by only changing one letter 
    // At the end of each level, increment steps by 1 
    // The process ends when target word is found in the queue
    
    // BFS ensures any solution we find is the shortest one.
    // (e.g.) We have two routes 1->2->3->4, 1->3->4. 
    // Our algorithm will add both 2 and 3 to queue in the first level, then find 4 in the 2nd level, thus ensuring the shortest route
    int ladderLength(string beginWord, string endWord, unordered_set<string>& wordList) {
        queue<string> nextWords; // BFS queue
        // Preprocess the word list for our convinience
        wordList.erase(beginWord);
        wordList.insert(endWord);
        // Add first level of words to the queue
        addNextWordToQueue(beginWord, wordList, nextWords);   
        int steps = 2;
        while(!nextWords.empty()){
            // Each for loop is considered as one level in the BFS
            // In each loop, retrieve all candidates in the queue, and apply BFS on each candidate
            int n = nextWords.size(); // ******MUST retrieve size of queue, b/c the size changes in the loop******
            for(int i = 0; i < n; i++){
                string candidate = nextWords.front();
                nextWords.pop();
                // Solution found
                if(candidate == endWord) return steps;
                // Early end
                if(wordList.empty()) continue;
                // Apply BFS on the candidate
                addNextWordToQueue(candidate, wordList, nextWords);
            }
            steps++;  
        }
        return 0;
    }
    
private:
    // Add valid next word to the BFS queue, and remove them from word list
    
    void addNextWordToQueue(string word, unordered_set<string>& wordList, queue<string>& nextWords){
        for(int i = 0; i < word.size(); i++){
            char temp = word[i];
            // Replace i-th char with a~z, apply BFS
            for(int j = 0; j < 26; j++){
                word[i] = 'a' + j;
                if(wordList.find(word) != wordList.end()){
                    nextWords.push(word);
                    wordList.erase(word);
                }
            }
            // Resume char before moving to next char
            word[i] = temp;
        }  
    }
    
};
```