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

###Another BFS Solution Using Union Find

We can use union find data structure to solve the problem.

The idea is to **union** all 'O' cells on boundaries to a dummy head, and **union** all neighboring 'O' cells.

Then we can tell that if an 'O' cell is surrounded by checking if it is connected to a dummy head.



---



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