# Union Find


The following tutorial gives a good explanation on concepts. **However**, its last implementation (find root) is not optimal. I will provide another version below.

https://www.cs.princeton.edu/~rs/AlgsDS07/01UnionFind.pdf

Also see my OneNote for more reference.


###Implementation and Optimization

We will implement an implicit tree which have **union** and **find** functions. As with all the trees, we will optimize the tree to make it as balanced as we can. 

*Optimization in union: *

```  
  Instead of union two nodes, we will union their roots. 
  (e.g.)We use unionTable[] to store root of subsets
  
  1. rootA = findroot(A)
  2. rootB = findroot(B)
  3. Apply logic if any, unionTable[rootA] = rootB or unionTable[rootB] = rootA```

Optimization in find: 

```
  Besides finding the root of the subset the node belongs, 
  we also update all the values in the subset to root 
  findRoot(uT[], int id)
    if(uT[id] == id) return id;
    uT[id] = findRoot(uT, uT[id]);
    return id;
  
  ```
  
Here is an alternative optimization in princeton's lecture which updates all values in the subset to its **grandfather**.

```
findRoot(uT[], int id)
  while(uT[id] != id)
    uT[id] = uT[uT[id]];
    id = uT[id];
  return id;
```





