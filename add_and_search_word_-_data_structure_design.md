# Add and Search Word - Data Structure Design


Design a data structure that supports the following two operations:

```
void addWord(word)
bool search(word)```
search(word) can search a literal word or a regular expression string containing only letters ```a-z``` or``` .```. 

A ```.``` means it can represent any one letter.

For example:

```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true```
Note:
You may assume that all words are consist of lowercase letters ```a-z```.

You should be familiar with how a **Trie** works. If not, please work on this problem: *2.9 Implement Trie (Prefix Tree)* first.



---




###The Code

```
// TrieNode represents a node in the WordDictionary class
class TrieNode{
public:
    TrieNode* children[26] = {nullptr};
    bool isLeaf;
    // Default constructor
    TrieNode():isLeaf(false){
    }
    
};

class WordDictionary {
public:

    // Default constructor
    WordDictionary(){
        root = new TrieNode();
    }
    
    // Destructor
    ~WordDictionary(){
        deleteHelper(root);
    }

    // Adds a word into the data structure.
    void addWord(string word) {
        TrieNode* node = root;
        int index = 0; // Index of each character. (e.g.) c has an index of 2
        for(int i = 0; i < word.size(); i++){
            index = word[i] - 'a';
            if(node->children[index] == nullptr){
                node->children[index] = new TrieNode();
            }
            node = node->children[index];
        }
        // Mark the last node as leaf
        node->isLeaf = true;
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    bool search(string word) {
        return searchHelper(word, root);
        
    }
private:
    TrieNode* root;
    // Helper function to search for a word
    // Parameters: search for the word starting from the node
    bool searchHelper(string word, TrieNode* node){
        int index = 0; // Index of each character. (e.g.) c has an index of 2
        for(int i = 0; i < word.size(); i++){
            index = word[i] - 'a';
            if(node && word[i] != '.'){
                node = node->children[index];
            }
            else if(node && word[i] == '.' && i != word.size()-1){ // When '.' is not the last char
                TrieNode* temp = node;
                for(int j = 0; j < 26; j++){
                    node = temp->children[j];
                    if( searchHelper(word.substr(i+1), node) ){ // If the word is found in any branch, return true
                        return true;
                    }
                }
            }
            else if(node && word[i] == '.' && i == word.size()-1){ // We can't call word.substr(i+1) when '.' is the last char
                for(int j = 0; j < 26; j++){ // If there is a char matches '.' and it is leaf, return true; otherwise, return false
                    if(node->children[j] && node->children[j]->isLeaf){
                        return true;
                    }
                }
                return false;
            }
            else{ // If a char is not in the trie, stop loop, no word found
                break;
            }
        }
        return node && node->isLeaf;
    }
    // Helper function to delete all nodes
    void deleteHelper(TrieNode* node){
        for(int i = 0; i < 26; i++){
            if(node->children[i]){
                deleteHelper(node->children[i]);
            }
        }
    }
};

// Your WordDictionary object will be instantiated and called as such:
// WordDictionary wordDictionary;
// wordDictionary.addWord("word");
// wordDictionary.search("pattern");
```


