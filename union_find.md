# Union Find


The following tutorial gives a good explanation on concepts. **However**, its last implementation (find root) is not optimal. I will provide another version below.

https://www.cs.princeton.edu/~rs/AlgsDS07/01UnionFind.pdf

Also see my OneNote for more reference.


##WUFPC -- Weighted Union Find Path Compression


---



We will implement a class (data structure) which has two functions, **union** and **find**.

Major features of the class include: **quick union, weighted tree, and pass impression**.

###Class Structure

Our class has two arrays. 

```
private:
  int id[]; // Records root of each node
  int sz[]; // Records size of each tree
```
In initialization, each node comprises a tree which only contains itself.

Initialization takes **O(n)** time.

```
WeightedUF(int n){
    for(int i = 0; i < n; i++){
      id[n] = n;
      sz[n] = 1;
  }
}```

Then we define a **find** function, in which we find the root of a node, **and** apply pass compression on all the nodes on the path.

Find will take at most **O(lg*N)** time. (lg* is read as log star, it is iterative log which grows extremely slow)
```
int find(int i){
      while(id[i] != i){ // On the path to find its root, 
        id[i] = id[id[i]]; // We update the node to point to its grandfather
        i = id[i];
      }
      return i;
}
```

In the **union** function, we append the root of a smaller tree to the root of a bigger tree, thus maintain a balanced tree. This is where the data structure gets its name "**weighted** UF".

Union function takes at most **O(lg*N)** time.

We will omit the proof here. Be practical!

```
void quickUnion(int i, int j){ // union is a keyword, so...
    int i = find(i);
    int j = find(j);
    if(sz[i] > sz[j]){ // If i is the larger tree
        id[j] = id[i]; // Append j to i
        sz[i] += sz[j]; // Update size of the larger tree (i)
    }
    else{
        id[i] = id[j];
        sz[j] += sz[i];
    }
}
```
Finally, we also have a **connected** function to check if two nodes are connected.

```
bool connected(int i, int j){
    return find(i) == find(j);
}

```


#Possible Applications


---


UF can be applied graph, social network, 2D grid, and more. 

We will list a summary of applications here, and provide details in each entry in this chapter.







