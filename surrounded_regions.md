# Surrounded Regions

Given a 2D board containing 'X' and 'O' (the letter O), capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

For example,
```
X X X X
X O O X
X X O X
X O X X
```
After running your function, the board should be:

```
X X X X
X X X X
X X X X
X O X X
```




---

###Why Breadth First Search

We can solve the problem using DFS. Actually, we will provide an elegant, and smart solution using DFS.

However, using DFS creates a large chain of recursion, which **potentially** causes **stack overflow** when the size of dimensions grows big enough. 

By the way, my first intuitive DFS solution passed all other cases except the biggest one, 250X250.

Before we go on, we should make sure we don't confuse between DFS and BFS.

The easiest way to **distinguish BFS** is that it does NOT go to next next until all next nodes are solved.

###A Normal BFS Solution

The idea behind is to solve the problem inversely -- identify what 'O' are not captured.

O on the borders are not surrounded, so as its neighboring O. 

Starting from borders, we find all such O, giving them temporary mark. 

The left O are thus surrounded by X, we flip them to X. 

Finally, we flip temporary mark back to O.

We provide both DFS and BFS solution which is based on this idea.

###BFS Solution Using Union Find

We can use union find data structure to solve the problem.

Main steps are as below:

**Compress 2D into 1D**:

Create an 1D array as union set, where ```row = index / width, col = index % width```.

Create a 1D bool array which represents whether an O node belongs to an open area or not.

**Initialization**:

Each node is the root of its own subset.

Set bool value of O nodes on boundaries to T.

**Union**:

Starting from bottom, union each node to its above node if they equal to each other.

Starting from left, union each node to its right node if they equal to each other.

**Flip Surrounded O**:

Loop through all O nodes, if bool value of ```Find(node)``` is False, flip it to X node. 

**Union Function**:
```
Union(Node A, Node B):
    RootA = Find(A)
    RootB = Find(B)
    unionSet(RootA) = RootB
    RootB.isOpen = RootA.isOpen || RootB.isOpen
```
**Find Function**: 

  We use **path compressing** in find function to optimize the union set.
  
  In ```Find(A)```, we update value of union set of all the nodes from ```A``` to ```rootA```. These values all become ```rootA``` so that next time we call ```find()``` on these nodes, time complexity is O(1).

```
Find(Node A)
  if(unionSet(A) == A) return A
  unionSet(A) = Find(unionSet(A))
  return unionSet(A)
```




---

###The Code



*DFS -- starting from borders.*

```
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        if(board.empty()) return;
        
        int rows = board.size();
        int cols = board[0].size();
        // Flip 'O' on four borders and its neighboring 'O' to 'N'
        // These 'O' cells are not surrounded
        for(int i = 0; i < rows; i++){
            flip(board, i, 0, rows, cols);
            if(cols > 1) flip(board, i, cols-1, rows, cols);  
        }
        for(int j = 0; j < cols; j++){
            flip(board, 0, j, rows, cols);
            if(rows > 1) flip(board, rows-1, j, rows, cols);  
        }
        // The left 'O' are surrounded, thus flip them to 'X'
        // Meanwhile, flip 'N' back to 'O'
        for(int i = 0; i < rows; i++){
            for(int j = 0; j < cols; j++){
                if(board[i][j]=='N') board[i][j] = 'O';
                else if(board[i][j] == 'O') board[i][j] = 'X';
            }
        }
    }
    
private:
    // Flip a 'O' cell and all of its neighboring 'O' cell to 'T'
    void flip(vector<vector<char>>& board, int i, int j, int rows, int cols){
        if(board[i][j] == 'O'){
            board[i][j] = 'N';
            // i > 1 , not i > 0, to avoid recheck border
            if(i > 1) flip(board, i-1, j, rows, cols);
            if(j > 1) flip(board, i, j-1, rows, cols);
            if(i < rows-1) flip(board, i+1, j, rows, cols);
            if(j < cols-1) flip(board, i, j+1, rows, cols);
            
        }
    }
};
```

*BFS -- starting from borders*

```
class Solution {
    
public:
void solve(vector<vector<char>>& board) {
	if (board.empty())  return;
	int m = board.size();
	int n = board[0].size();
	queue<pair<int, int>> q;
	for (int i = 0; i < m; i++){
		if (board[i][0] == 'O') bfs(board, i, 0, q);
		if (board[i][n - 1] == 'O') bfs(board, i, n - 1, q);
	}
	for (int j = 0; j < n; j++){
		if (board[0][j] == 'O') bfs(board, 0, j, q);
		if (board[m - 1][j] == 'O') bfs(board, m - 1, j, q);
	}

	for (int i = 0; i < m; i++){
		for (int j = 0; j < n; j++){
			if (board[i][j] == 'O') board[i][j] = 'X';
			else if (board[i][j] == 'B') board[i][j] = 'O';
		}
	}
}
private:
void bfs(vector<vector<char>>& board, int row, int col, queue<pair<int, int>>& q){
	if(board[row][col] != 'O') return;
	board[row][col] = 'B';
	q.push(make_pair(row, col));
	int m = board.size();
	int n = board[0].size();
	while (!q.empty()){
		auto next = q.front();
		q.pop();
		int i = next.first, j = next.second;
		//board[i][j] = 'B';
		if (i > 0 && board[i - 1][j] == 'O'){
		    board[i - 1][j] = 'B';
			q.push(make_pair(i - 1, j));
		}
		if (i<m - 1 && board[i + 1][j] == 'O'){
		    board[i+1][j] = 'B';
			q.push(make_pair(i + 1, j));
		}
		if (j>0 && board[i][j - 1] == 'O'){
		    board[i][j-1] = 'B';
			q.push(make_pair(i, j - 1));
		}
		if (j < n - 1 && board[i][j + 1] == 'O'){
		    board[i][j+1] = 'B';
			q.push(make_pair(i, j + 1));
		}
	}
}
};
```

*BFS --  Union Find*

We can still improve this solution by introducing a struct which **encapsulates** the union set data structure.

```
class Solution {

vector<int> unionSet; // 1D array representing cells
vector<bool> isOpen; // 1D array representing if a cell is in an open area

public:
    void solve(vector<vector<char>>& board) {
        if(board.empty()) return;
        int rows = board.size();
        int cols = board[0].size();
        
        unionSet.resize(rows*cols);
        isOpen.resize(rows*cols);
        // Initialization:
        // Put each cell to a separate subset of which root is itself
        // Set isOpen of O cells on boundaries to true
        for(int i = 0; i < rows*cols; i++){
            unionSet[i] = i;
            int row = i / cols;
            int col = i % cols;
            if((row == 0 && board[row][col] == 'O') || (row == rows - 1 && board[row][col] == 'O')
                || (col == 0 && board[row][col] == 'O') || (col == cols - 1 && board[row][col] == 'O')){
                    isOpen[i] = true;
                }
        }
        
        // Union:
        // From bottom, union each O cell to the cell to its above if they have same value
        for(int j = 0; j < cols; j++){
            for(int i = rows - 1; i > 0; i--){
                if((board[i][j] == 'O') && (board[i][j] == board[i-1][j])){
                    unionTwoCells(i*cols + j, (i-1)*cols + j);
                }
            }
        }
        // From left, union each O cell to the cell to its right if they have same value
        for(int i = 0; i < rows; i++){
            for(int j = 0; j < cols - 1; j++){
                if((board[i][j] == 'O') && board[i][j] == board[i][j+1]){
                    unionTwoCells(i*cols + j, i*cols + j + 1);
                }
            }
        }
        
        // Flip:
        // Flip all O cells that are not in an open area
        for(int i = 0; i < rows*cols; i++){
            int row = i / cols;
            int col = i % cols;
            // Check isOpen of root cell to see whether the subset is open or surrounded
            if(board[row][col] == 'O' && isOpen[findCell(i)] == false){
                board[row][col] = 'X';
            }
        }
    }
    
private:
    // Union two cells
    // a,b : index of cells in the 1D array
    // Each time we union two cells, we update isOpen of root cell of the new subset
    void unionTwoCells(int a, int b){
        int rootA = findCell(a);
        int rootB = findCell(b);
        unionSet[rootA] = rootB;
        isOpen[rootB] = isOpen[rootA] || isOpen[rootB];
    }
    
    // Find root for a cell
    // a: index of a cell in the 1D array
    int findCell(int a){
        if(unionSet[a] == a) return a; // a is root of its subset
        unionSet[a] = findCell(unionSet[a]); // path compression
        return unionSet[a];
    }
    
};
```