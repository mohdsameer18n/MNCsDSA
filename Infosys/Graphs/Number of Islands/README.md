# Number of Islands (LeetCode 200)

## Problem Statement

Given an `m x n` 2D binary grid `grid` where:

- `'1'` represents **Land**
- `'0'` represents **Water**

Return the **number of islands**.

An island is formed by connecting adjacent lands **horizontally or vertically**. You may assume all four edges of the grid are surrounded by water.

---

## Example

### Input

```text
grid =
[
  ['1','1','0','0','0'],
  ['1','1','0','0','0'],
  ['0','0','1','0','0'],
  ['0','0','0','1','1']
]
```

### Output

```text
3
```

### Explanation

There are **3 separate groups** of connected land.

```text
Island 1

1 1
1 1

Island 2

    1

Island 3

      1 1
```

---

# Approach (DFS)

We scan every cell of the grid.

- If we find a `'1'`, it means we discovered a **new island**.
- Increment the island count.
- Perform **DFS** to visit all connected land cells.
- During DFS, change every visited `'1'` to `'0'` so it won't be counted again.

This ensures every island is counted exactly once.

---

# Algorithm

1. Traverse every cell of the grid.
2. If the current cell is `'1'`:
   - Increment `islands`.
   - Call `dfs(row, col)`.
3. In DFS:
   - Return if the cell is out of bounds or already water (`'0'`).
   - Mark the current land as visited by changing `'1'` to `'0'`.
   - Visit all four directions:
     - Up
     - Down
     - Left
     - Right
4. Return the total number of islands.

---

# Dry Run

### Input

```text
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

Initially

```text
islands = 0
```

### Step 1

Cell `(0,0)` = `'1'`

```text
islands = 1
```

DFS visits

```text
(0,0)
(0,1)
(1,0)
(1,1)
```

Grid becomes

```text
0 0 0 0 0
0 0 0 0 0
0 0 1 0 0
0 0 0 1 1
```

---

### Step 2

Continue scanning.

Cell `(2,2)` = `'1'`

```text
islands = 2
```

DFS visits

```text
(2,2)
```

Grid becomes

```text
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 1 1
```

---

### Step 3

Continue scanning.

Cell `(3,3)` = `'1'`

```text
islands = 3
```

DFS visits

```text
(3,3)
(3,4)
```

Final Grid

```text
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
```

Traversal completed.

Return

```text
3
```

---

# Java Solution

```java
class Solution {

    public int numIslands(char[][] grid) {

        int n = grid.length;
        int m = grid[0].length;

        int islands = 0;

        for (int i = 0; i < n; i++) {

            for (int j = 0; j < m; j++) {

                if (grid[i][j] == '1') {

                    islands++;

                    dfs(grid, i, j);
                }
            }
        }

        return islands;
    }

    private void dfs(char[][] grid, int row, int col) {

        if (row < 0 || col < 0 ||
            row >= grid.length ||
            col >= grid[0].length ||
            grid[row][col] == '0') {

            return;
        }

        // Mark current land as visited
        grid[row][col] = '0';

        // Explore all four directions
        dfs(grid, row + 1, col); // Down
        dfs(grid, row - 1, col); // Up
        dfs(grid, row, col + 1); // Right
        dfs(grid, row, col - 1); // Left
    }
}
```

---

# Complexity Analysis

### Time Complexity

```
O(M × N)
```

- Every cell is visited at most once.

### Space Complexity

```
O(M × N)
```

- Worst-case recursion stack when the entire grid is one large island.

---

# Key Points

- Treat each island as a **connected component**.
- Every time an unvisited `'1'` is found, increment the island count.
- Use **DFS** to mark the entire connected island as visited.
- Mark visited cells as `'0'` to avoid counting the same island again.
- Only **up, down, left, and right** directions are considered (no diagonal connections).

---

# Similar Problems

- 695. Max Area of Island
- 733. Flood Fill
- 130. Surrounded Regions
- 994. Rotting Oranges
- 463. Island Perimeter
