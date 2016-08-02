# Implement Trie(Prefix Tree)

Implement a trie with insert, search, and startsWith methods.

Note:
You may assume that all inputs are consist of lowercase letters a-z.



---



# Trie


https://en.wikipedia.org/wiki/Trie

A trie, also known as a prefix tree, is an ordered tree that store (usually) strings.

All the descendants of a node have the same prefix.


### Properties of Trie



1. The root is an empty node, 
2. Each node has [Alphabet_Size] children. 
(e.g.) If chars allowed are a~z, root has 26 child nodes.
3. Value is not associated with the char, but used to distinguish the leaf node.

4. A leaf node is where a word ends. e.g. in the following trie, both e(*) and m(*) are leaf nodes since both "the" and "them" are words.
```
        root
        /  \
       t    e
      / \    \
     h   o(*) a
    /          \  
   e(*)         t(*)
  /
 m(*) 
```

### Insert and Search

Both insert and search takes time complexity O(length_of_word).

Using the index of child nodes, action at each level takes O(1) time.

(e.g.) Adding "ace" to our tree requires three steps: root->child[0]->child[2]->child[4]




### Comparing to BST


A well balanced BST's time complexity is O(M*$$logN$$), while M is the length of the string and N is the total keys stored in the tree.

A trie always takes O(M) no matter how large the storage is.


The shorage of trie is the vast requirement of storage space due to its property. (miltiple child nodes)



---


Code is as below. Without destructor, run time is 58ms, while with destructor, it takes 72ms.


```
class TrieNode {
public:
    TrieNode* children[26] = {nullptr};
    bool isLeaf;
    // Initialize your data structure here.
    TrieNode() : isLeaf(false) {
        
        
    }
};

class Trie {
public:
    Trie() {
        root = new TrieNode();
    }
    
    ~Trie()
    {
        if(root)
        {
            deleteHelper(root);
        }
    }

    // Inserts a word into the trie.
    void insert(string word) 
    {
        TrieNode* node = root;
        int index = 0; // Index of each alphabelt in the string
        for(int i = 0; i < word.length(); i++)
        {
            // word[i] is ASCII code
            // Remove the offset of 'a' to realign index to 0~25
            index = word[i] - 'a';
            // If the string is not present, insert; otherwise only mark last node as leaf
            if(node->children[index] == nullptr)
            {
                node->children[index] = new TrieNode();
            }
            node = node->children[index];
        }
        // Mark last node as leaf
        node->isLeaf = true;
    }

    // Returns if the word is in the trie.
    bool search(string word) 
    {
        TrieNode* node = root;
        int index = 0;
        for(int i = 0 ; i < word.length(); i++)
        {
            index = word[i] - 'a';
            if(node->children[index] == nullptr)
            {
                return false;
            }
            node = node->children[index];
        }
        // In addition to check nullptr, also check if the last node is leaf 
        // This step distinguishs searching for a word from a prefix
        return (node != nullptr && node->isLeaf);
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    bool startsWith(string prefix) 
    {
        TrieNode* node = root;
        int index = 0;
        for(int i = 0 ; i < prefix.length(); i++)
        {
            index = prefix[i] - 'a';
            if(node->children[index] == nullptr)
            {
                return false;
            }
            node = node->children[index];
        }
        return (node != nullptr);
    }

private:
    TrieNode* root;
    
    void deleteHelper(TrieNode* node)
    {
        for(int i = 0; i < 26; i++)
        {
            if(node->children[i])
            {
                deleteHelper(node->children[i]);
            }
        }
        delete node;
    }
};



// Your Trie object will be instantiated and called as such:
// Trie trie;
// trie.insert("somestring");
// trie.search("key");
```



