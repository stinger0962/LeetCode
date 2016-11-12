# Find the Celebrity

Suppose you are at a party with n people (labeled from 0 to n - 1) and among them, there may exist one celebrity. The definition of a celebrity is that all the other n - 1 people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function ```bool knows(a, b)``` which tells you whether A knows B. Implement a function ```int findCelebrity(n)```, your function should minimize the number of calls to knows.

Note: There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return -1.




---


###First Attempt

At first, I think this is a graph, double-direction graph problem.

Intuitively, I approached by a breadth-first-search algorithm. 

It's not hard to get to the destination, but the O(NXN) algorithm is not efficient.


###Code

```
// Forward declaration of the knows API.
bool knows(int a, int b);

class Solution {
public:
    int findCelebrity(int n) {
        vector<char> status(n, 'U'); // record status of all people 'U' unknown 'N' not celebrity
        int cel = -1; // index of the celebrity
        // breadth first search for each person 
        for (int i = 0; i < n; i++) {
            if (status[i] == 'N') continue; // skip a person who has been identified as normal
            for (int j = 0; j <= n; j++) {
                if (j == n) { // this only happens when a celeb is found
                    cel = i;
                    return cel;
                }
                if (i == j) continue; // skip validate self
                if (!knows(j, i) || knows(i, j)) { // conditions that i disqualified to be a celeb
                    status[i] == 'N';
                    break;
                }
            }
        }
        return cel;
    }
};
```

###Exact One Candidate

A smart solution involves to take advantage of the giving infomation that there is at most ONE celebrity.

It finds a exclusive candidate from the first loop, then validate the candite in the second loop.

Time complexity: O(N) 

```
// Forward declaration of the knows API.
bool knows(int a, int b);

class Solution {
public:
    int findCelebrity(int n) {
        int cad = 0; // index of the candidate
        // find the only candidate
        // why this works:
        // 1. if knows(cad, i), then cad cannot be celebrity
        // 2. for those j: b/w cad and i, since !knows(cad, j), j also disqualified
        // 3. for those k: b/w cad and n-1, since !knows(cad, k), k also disqualified
        // this loop seems trivial, but very smart indeed
        for (int i = 1; i < n; i++) {
            if (knows(cad, i)) cad = i;
        }
        // now we verify the only candidate
        for (int i = 0; i < n; i++) {
            if (i < cad && ( knows(cad, i) || !knows(i, cad))) return -1; // for i < cad, we check both direction
            if (i > cad && !knows(i, cad)) return -1; // for i > cad, we know cad->i is false from the last loop
        }
        return cad;
    }
};
```

