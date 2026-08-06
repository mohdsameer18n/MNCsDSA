# 130. Surrounded Regions

## Problem Statement

You are given an `m × n` board containing characters:

- `'X'` → Wall
- `'O'` → Empty region

Capture all **surrounded regions** by changing every `'O'` that is completely surrounded by `'X'` into `'X'`.

A region is **surrounded** if it is **not connected to any boundary `'O'`**.

The board must be modified **in-place**.

---

## Example

### Input

```text
X X X X
X O O X
X X O X
X O X X
```

### Output

```text
X X X X
X X X X
X X X X
X O X X
```

---

## Explanation

The region

```text
O O
  O
```

is completely surrounded by `'X'`, so it becomes `'X'`.

The `'O'` at the boundary

```text
(3,1)
```

cannot be surrounded, so it remains `'O'`.

---

# Intuition

Instead of finding the surrounded regions directly, find the regions that **cannot** be surrounded.

Any `'O'` connected to the boundary is always safe.

So,

1. Start DFS from every boundary `'O'`.
2. Mark all connected `'O'` cells as safe (`'T'`).
3. Convert all remaining `'O'` to `'X'`.
4. Restore `'T'` back to `'O'`.

---

# Approach

### Step 1

Traverse all boundary cells.

```text
Top row
Bottom row
Left column
Right column
```

Whenever you find an `'O'`, start DFS.

---

### Step 2

During DFS,

Mark

```text
'O' → 'T'
```

so that it is not flipped later.

---

### Step 3

Traverse the board again.

If

```text
'O'
```

then

```text
'O' → 'X'
```

because it is surrounded.

If

```text
'T'
```

restore

```text
'T' → 'O'
```

---

# Algorithm

```
Traverse boundary

For every boundary 'O'

    DFS

DFS

    if out of boundary
        return

    if cell is not 'O'
        return

    mark current cell as 'T'

    visit 4 neighbours

Traverse board

if 'O'
    convert to 'X'

if 'T'
    convert back to 'O'
```

---

# Dry Run

### Input

```text
X X X X
X O O X
X X O X
X O X X
```

Coordinates

```text
      0 1 2 3
    +---------
0 |  X X X X
1 |  X O O X
2 |  X X O X
3 |  X O X X
```

---

## Boundary Traversal

Boundary `'O'`

```text
(3,1)
```

Run DFS.

Mark

```text
T
```

Board

```text
X X X X
X O O X
X X O X
X T X X
```

---

No other boundary `'O'`.

---

## Traverse Entire Board

Remaining `'O'`

```text
(1,1)
(1,2)
(2,2)
```

They are not connected to boundary.

Convert

```text
O → X
```

Board

```text
X X X X
X X X X
X X X X
X T X X
```

---

Restore

```text
T → O
```

Final Board

```text
X X X X
X X X X
X X X X
X O X X
```

---

# Java Solution

```java
class Solution {

    int[][] directions = {
        {0,1},
        {1,0},
        {0,-1},
        {-1,0}
    };

    private void dfs(int r, int c, char[][] board){

        int m = board.length;
        int n = board[0].length;

        if(r < 0 || c < 0 || r >= m || c >= n || board[r][c] != 'O'){
            return;
        }

        board[r][c] = 'T';

        for(int[] dir : directions){

            int nr = r + dir[0];
            int nc = c + dir[1];

            dfs(nr, nc, board);
        }
    }

    public void solve(char[][] board) {

        int m = board.length;
        int n = board[0].length;

        if(m == 0 || n == 0){
            return;
        }

        // First & Last Column
        for(int i = 0; i < m; i++){

            if(board[i][0] == 'O')
                dfs(i, 0, board);

            if(board[i][n-1] == 'O')
                dfs(i, n-1, board);
        }

        // First & Last Row
        for(int j = 0; j < n; j++){

            if(board[0][j] == 'O')
                dfs(0, j, board);

            if(board[m-1][j] == 'O')
                dfs(m-1, j, board);
        }

        // Convert Board
        for(int i = 0; i < m; i++){

            for(int j = 0; j < n; j++){

                if(board[i][j] == 'O'){
                    board[i][j] = 'X';
                }
                else if(board[i][j] == 'T'){
                    board[i][j] = 'O';
                }
            }
        }
    }
}
```

---

# Why Start DFS from Boundary?

Suppose the board is

```text
X X X X
X O O X
X X O X
X O X X
```

The boundary `'O'`

```text
(3,1)
```

can never be surrounded.

So we first mark it as safe.

After marking all safe cells, every remaining `'O'` must be surrounded.

---

# Complexity Analysis

### Time Complexity

```
O(m × n)
```

Each cell is visited at most once.

---

### Space Complexity

```
O(m × n)
```

Worst-case recursion stack when the board contains only `'O'`.

---

# Pattern Recognition

This problem belongs to:

- DFS on Grid
- BFS on Grid
- Flood Fill
- Graph Traversal
- Connected Components
- Boundary DFS

---

# Key Observations

- Do **not** start DFS from every `'O'`.
- Start only from **boundary `'O'`** cells.
- Mark boundary-connected cells as `'T'`.
- Remaining `'O'` cells are surrounded.
- Restore `'T'` back to `'O'`.

---

# Similar Problems

| Problem | Pattern |
|----------|---------|
| 200. Number of Islands | DFS / BFS |
| 695. Max Area of Island | DFS |
| 733. Flood Fill | DFS / BFS |
| 1020. Number of Enclaves | Boundary DFS |
| 1254. Number of Closed Islands | DFS |
| 417. Pacific Atlantic Water Flow | Boundary DFS |
| 1905. Count Sub Islands | DFS |
| 994. Rotting Oranges | Multi-Source BFS |
| 1306. Jump Game III | DFS / BFS |
| 529. Minesweeper | DFS / BFS |

---

# Interview Tips

- Think in reverse: instead of finding surrounded regions, find **safe** regions.
- Boundary-connected `'O'` cells are never flipped.
- Use DFS/BFS to mark safe cells.
- Use a temporary marker (`'T'`) to distinguish safe cells.
- Modify the board in-place to achieve **O(1)** extra space (excluding recursion stack).
