# 1905. Count Sub Islands

## Problem Statement

You are given two binary matrices:

- `grid1`
- `grid2`

where:

- `1` → Land
- `0` → Water

An island is a group of connected land cells connected **horizontally or vertically**.

A **sub-island** is an island in `grid2` where **every land cell also exists as land in `grid1`**.

Return the **number of sub-islands**.

---

## Example

### Input

```text
grid1 =
[
 [1,1,1,0,0],
 [0,1,1,1,1],
 [0,0,0,0,0],
 [1,0,0,0,0],
 [1,1,0,1,1]
]

grid2 =
[
 [1,1,1,0,0],
 [0,0,1,1,1],
 [0,1,0,0,0],
 [1,0,1,1,0],
 [0,1,0,1,0]
]
```

### Output

```text
3
```

---

# Intuition

Instead of checking every cell individually, we process **one complete island of `grid2` at a time**.

While traversing an island:

- If every land cell also exists in `grid1`, the island is a **sub-island**.
- If even one land cell corresponds to water in `grid1`, the entire island is **not** a sub-island.

---

# Approach

1. Traverse every cell of `grid2`.
2. Whenever a land cell (`1`) is found:
   - Start DFS.
   - Visit the entire island.
   - Check whether every visited cell is also land in `grid1`.
3. If DFS returns `true`, increment the answer.

---

# DFS Logic

### Base Cases

If

```text
Outside grid
```

or

```text
Current cell is water
```

return

```text
true
```

because these situations do **not** invalidate a sub-island.

---

### Current Cell

```java
boolean isSub = (grid1[r][c] == 1);
```

If

```text
grid1[r][c] = 0
grid2[r][c] = 1
```

then

```text
isSub = false
```

because this land cell doesn't exist in `grid1`.

Mark the current cell as visited.

```java
grid2[r][c] = 0;
```

---

### Visit All Four Directions

```java
for(int[] dir : directions){

    int nr = r + dir[0];
    int nc = c + dir[1];

    isSub = dfs(nr, nc, grid1, grid2) && isSub;
}
```

---

# Why `dfs(...) && isSub` ?

This is the most important part.

Suppose

```text
Current cell = false
```

If we write

```java
isSub = isSub && dfs(...);
```

Java uses **short-circuit evaluation**.

Since `isSub` is already `false`, `dfs()` is **never called**.

As a result,

- Some cells remain unvisited.
- The same island gets counted again later.

Wrong Answer.

Instead use

```java
isSub = dfs(...) && isSub;
```

Now `dfs()` executes first, ensuring the **entire island is visited** before combining the boolean result.

---

# Dry Run

### grid1

```text
1 1
0 1
```

### grid2

```text
1 1
1 1
```

Start DFS from

```text
(0,0)
```

DFS Tree

```text
dfs(0,0)
    |
    +------ dfs(0,1)
                |
                +------ dfs(1,1)
                            |
                            +------ dfs(1,0)
```

---

## At (1,0)

```text
grid1 = 0

grid2 = 1
```

So

```java
isSub = false;
```

Return

```text
false
```

---

## Backtracking

At `(1,1)`

```java
isSub = dfs(1,0) && true;

false && true

=

false
```

Return

```text
false
```

At `(0,1)`

```text
false && true

=

false
```

Return

```text
false
```

At `(0,0)`

```text
false && true

=

false
```

Entire island is **not** a sub-island.

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

    private boolean dfs(int r, int c, int[][] grid1, int[][] grid2){

        int m = grid1.length;
        int n = grid1[0].length;

        // Base Case
        if(r < 0 || c < 0 || r >= m || c >= n || grid2[r][c] == 0){
            return true;
        }

        // Mark visited
        grid2[r][c] = 0;

        // Check current cell
        boolean isSub = (grid1[r][c] == 1);

        // Visit all neighbours
        for(int[] dir : directions){

            int nr = r + dir[0];
            int nc = c + dir[1];

            isSub = dfs(nr, nc, grid1, grid2) && isSub;
        }

        return isSub;
    }

    public int countSubIslands(int[][] grid1, int[][] grid2) {

        int m = grid1.length;
        int n = grid1[0].length;

        int count = 0;

        for(int i = 0; i < m; i++){

            for(int j = 0; j < n; j++){

                if(grid2[i][j] == 1){

                    if(dfs(i, j, grid1, grid2)){
                        count++;
                    }
                }
            }
        }

        return count;
    }
}
```

---

# Complexity Analysis

### Time Complexity

```
O(m × n)
```

Every cell of `grid2` is visited exactly once.

---

### Space Complexity

```
O(m × n)
```

Worst-case recursion stack when the entire grid is land.

---

# Key Observations

- Traverse **only islands in `grid2`**.
- Every land cell must also exist in `grid1`.
- Continue DFS even after finding an invalid cell to mark the whole island as visited.
- Use

```java
dfs(...) && isSub
```

instead of

```java
isSub && dfs(...)
```

to avoid Java's short-circuit skipping recursive DFS calls.

---

# Pattern Recognition

This problem belongs to:

- DFS on Grid
- Flood Fill
- Connected Components
- Boolean DFS
- Graph Traversal

---

# Similar Problems

| Problem | Pattern |
|----------|---------|
| 200. Number of Islands | DFS / BFS |
| 695. Max Area of Island | DFS |
| 130. Surrounded Regions | Boundary DFS |
| 1254. Number of Closed Islands | DFS |
| 1020. Number of Enclaves | DFS |
| 733. Flood Fill | DFS |
| 463. Island Perimeter | DFS |
| 417. Pacific Atlantic Water Flow | DFS/BFS |
| 827. Making A Large Island | DFS + Union Find |

---

# Interview Tips

- Think of each island in `grid2` as one connected component.
- DFS returns a boolean indicating whether the explored island is a valid sub-island.
- Mark cells as visited immediately.
- Be careful with Java's `&&` short-circuit behavior:
  - ❌ `isSub = isSub && dfs(...)`
  - ✅ `isSub = dfs(...) && isSub`
- One invalid land cell makes the entire island invalid.
