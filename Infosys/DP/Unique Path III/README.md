# Unique Paths III

## Problem Statement

You are given an `m x n` grid where:

- `1` → Starting square (exactly one)
- `2` → Ending square (exactly one)
- `0` → Empty square that can be walked on
- `-1` → Obstacle that cannot be walked on

Find the number of unique paths from the **start** to the **end** such that:

- Every non-obstacle square is visited **exactly once**.
- You can move only in **4 directions**:
  - Up
  - Down
  - Left
  - Right

---

## Example

### Input

```text
grid =
[
 [1,0,0,0],
 [0,0,0,0],
 [0,0,2,-1]
]
```

### Output

```text
2
```

### Valid Paths

```text
Path 1

(0,0)
→
(0,1)
→
(0,2)
→
(0,3)
↓
(1,3)
←
(1,2)
←
(1,1)
←
(1,0)
↓
(2,0)
→
(2,1)
→
(2,2)
```

```text
Path 2

(0,0)
↓
(1,0)
↓
(2,0)
→
(2,1)
↑
(1,1)
↑
(0,1)
→
(0,2)
→
(0,3)
↓
(1,3)
←
(1,2)
↓
(2,2)
```

---

# Approach

This problem is solved using **DFS + Backtracking**.

Unlike **Unique Paths I & II**, this problem cannot be solved using normal DP because the answer depends on **which cells have already been visited**, not just the current position.

---

## Step 1: Count Walkable Cells

Before starting DFS:

- Count every cell except `-1`.
- Find the starting cell.

Example:

```text
1 0 0 0
0 0 0 0
0 0 2 X
```

Walkable cells

```text
1 Start
8 Empty
1 End
-----------
Total = 11
```

Store

```java
total = 11;
```

Also store

```java
startRow
startCol
```

---

## Step 2: Start DFS

Start from the starting square.

```java
dfs(startRow, startCol, 1);
```

`1` means the starting square has already been visited.

---

## DFS Algorithm

At every recursive call:

### 1. Boundary Check

```java
if(r<0 || c<0 || r>=m || c>=n)
    return;
```

---

### 2. Obstacle or Already Visited

```java
if(grid[r][c]==-1 || visited[r][c])
    return;
```

---

### 3. Reached End

```java
if(grid[r][c]==2)
```

If every walkable cell has already been visited,

```java
if(count==total)
    ans++;
```

Otherwise

```java
return;
```

---

### 4. Mark Current Cell

```java
visited[r][c]=true;
```

---

### 5. Explore All Directions

```java
dfs(r+1,c,count+1);

dfs(r-1,c,count+1);

dfs(r,c+1,count+1);

dfs(r,c-1,count+1);
```

---

### 6. Backtrack

```java
visited[r][c]=false;
```

This allows the same cell to be used in another possible path.

---

# Dry Run

Input

```text
1 0 0 0
0 0 0 0
0 0 2 X
```

Start

```text
dfs(0,0,1)
```

Visited

```text
✓ . . .
. . . .
. . E X
```

Go Down

```text
✓ . . .
✓ . . .
. . E X
```

Go Down

```text
✓ . . .
✓ . . .
✓ . E X
```

Go Right

```text
✓ . . .
✓ . . .
✓ ✓ E X
```

If DFS immediately reaches End

```text
count = 5
total = 11
```

Since

```text
5 != 11
```

Return.

This path is invalid.

Backtrack.

Try another direction.

Eventually DFS reaches

```text
✓ ✓ ✓ ✓
✓ ✓ ✓ ✓
✓ ✓ E X
```

Now

```text
count = 11
```

Move to End.

```text
11 == total
```

Answer becomes

```text
ans = 1
```

Backtracking continues until another valid path is found.

Finally

```text
ans = 2
```

---

# Recursion Tree

```text
dfs(Start)
│
├── Down
│     │
│     ├── Down
│     │      │
│     │      ├── Right
│     │      │      │
│     │      │      ├── End ❌
│     │      │      │
│     │      │      └── Up
│     │      │             │
│     │      │             └── ...
│     │      │                    │
│     │      │                    └── End ✔
│     │
│     └── Other branches
│
└── Right
      │
      └── ...
             │
             └── End ✔
```

DFS explores one complete path before trying another.

---

# Why Backtracking?

Suppose we never write

```java
visited[r][c]=false;
```

After one path finishes,

```text
✓ ✓ ✓ ✓
✓ ✓ ✓ ✓
✓ ✓ E X
```

All cells remain visited.

When DFS tries another route,

it immediately returns because

```java
visited[r][c]==true
```

The second valid path is never explored.

Therefore,

```java
visited[r][c]=false;
```

is necessary.

---

# Java Solution

```java
class Solution {

    int ans = 0;
    int total = 0;

    int[][] dir = {
        {1,0},
        {-1,0},
        {0,1},
        {0,-1}
    };

    public void dfs(int r,int c,int count,int[][] grid,boolean[][] visited){

        int m = grid.length;
        int n = grid[0].length;

        if(r<0 || c<0 || r>=m || c>=n)
            return;

        if(grid[r][c]==-1 || visited[r][c])
            return;

        if(grid[r][c]==2){
            if(count==total)
                ans++;
            return;
        }

        visited[r][c]=true;

        for(int[] d:dir){
            dfs(r+d[0], c+d[1], count+1, grid, visited);
        }

        visited[r][c]=false;
    }

    public int uniquePathsIII(int[][] grid) {

        int m = grid.length;
        int n = grid[0].length;

        int sr=0;
        int sc=0;

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){

                if(grid[i][j]!=-1)
                    total++;

                if(grid[i][j]==1){
                    sr=i;
                    sc=j;
                }
            }
        }

        boolean[][] visited = new boolean[m][n];

        dfs(sr,sc,1,grid,visited);

        return ans;
    }
}
```

---

# Complexity Analysis

### Time Complexity

Worst case:

```text
O(4^K)
```

where `K` is the number of walkable cells.

---

### Space Complexity

```text
O(K)
```

- Recursion stack
- `visited[][]` array

---

# Key Takeaways

- This is a **DFS + Backtracking** problem.
- Visit every non-obstacle cell exactly once.
- Mark the current cell as visited before exploring.
- Unmark it after exploring (backtracking).
- Count a path only when reaching the end **after visiting all walkable cells**.
- Normal DP (`dp[row][col]`) does not work because the visited cells affect future choices.
