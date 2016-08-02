# implement Trie(Prefix Tree)

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
(e.g.) If chars allowed are a~z, root has 26 children.
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




