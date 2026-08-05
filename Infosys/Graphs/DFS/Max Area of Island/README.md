# 695. Max Area of Island

## Problem Statement

You are given an `m x n` binary matrix `grid` where:

- `1` represents **land**
- `0` represents **water**

An **island** is formed by connecting adjacent lands **horizontally or vertically**.

Return the **maximum area** of an island in the grid. If there is no island, return `0`.

---

## Example

### Input

```text
grid =
[
 [0,0,1,0,0],
 [1,1,1,0,1],
 [0,1,0,0,1],
 [0,0,0,1,1]
]
```

### Output

```text
5
```

### Explanation

The largest island contains the following cells:

```text
      0 1 2 3 4
    +-----------
0 |  0 0 X 0 0
1 |  X X X 0 1
2 |  0 X 0 0 1
3 |  0 0 0 1 1
```

Area = **5**

---

# Intuition

Treat every land cell (`1`) as a node in a graph.

Whenever we encounter an unvisited land cell:

- Start a DFS.
- Visit every connected land cell.
- Count the number of cells visited.
- Update the maximum area.

Since every land cell belongs to exactly one island, every cell is visited only once.

---

# Approach (DFS)

1. Traverse every cell in the grid.
2. If the current cell is land (`1`):
   - Start DFS.
   - Count the size of the connected island.
3. Mark every visited land as `0` to avoid revisiting.
4. Update the maximum island area.
5. Return the maximum area.

---

# Algorithm

```
maxArea = 0

for every cell:
    if cell == 1
        area = DFS(cell)
        maxArea = max(maxArea, area)

return maxArea
```

DFS:

```
DFS(r,c)

if out of boundary or water
    return 0

mark current cell visited

area = 1

visit
    up
    down
    left
    right

return total area
```

---

# Dry Run

### Input

```text
1 1 0
1 0 1
0 1 1
```

Initially

```text
maxArea = 0
```

### First Island

Start DFS from `(0,0)`.

Visited cells

```text
(0,0)
(0,1)
(1,0)
```

Area

```text
3
```

Update

```text
maxArea = 3
```

---

### Continue Traversal

Start DFS from `(1,2)`.

Visited cells

```text
(1,2)
(2,2)
(2,1)
```

Area

```text
3
```

Update

```text
maxArea = max(3,3)

= 3
```

Answer

```text
3
```

---

# DFS Recursion Tree

Example

```text
1 1
1 1
```

```
dfs(0,0)
│
├── dfs(1,0)
│
├── dfs(-1,0)
│
├── dfs(0,1)
│
└── dfs(0,-1)
```

Every recursive call returns the size of the connected island rooted at that cell.

---

# Java Solution

```java
class Solution {

    int[][] dir = {
        {1,0},
        {-1,0},
        {0,1},
        {0,-1}
    };

    public int maxAreaOfIsland(int[][] grid) {

        int m = grid.length;
        int n = grid[0].length;

        int maxArea = 0;

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {

                if(grid[i][j] == 1) {
                    maxArea = Math.max(maxArea, dfs(i, j, grid));
                }
            }
        }

        return maxArea;
    }

    private int dfs(int r, int c, int[][] grid) {

        int m = grid.length;
        int n = grid[0].length;

        if(r < 0 || c < 0 || r >= m || c >= n || grid[r][c] == 0)
            return 0;

        grid[r][c] = 0;

        int area = 1;

        for(int[] d : dir) {
            area += dfs(r + d[0], c + d[1], grid);
        }

        return area;
    }
}
```

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

Worst-case recursion stack when the entire grid is one large island.

---

# Key Observations

- Grid problems can often be modeled as graphs.
- Every island is a connected component.
- DFS/BFS explores one connected component at a time.
- Mark visited cells immediately to avoid infinite recursion.
- Modifying the grid (`grid[r][c] = 0`) eliminates the need for a separate `visited` array.

---

# Pattern Recognition

This problem belongs to:

- DFS on Grid
- BFS on Grid
- Flood Fill
- Connected Components
- Graph Traversal

---

# Similar Problems

| Problem | Pattern |
|----------|---------|
| 200. Number of Islands | DFS / BFS |
| 733. Flood Fill | DFS / BFS |
| 463. Island Perimeter | DFS |
| 1254. Number of Closed Islands | DFS |
| 1020. Number of Enclaves | DFS |
| 1905. Count Sub Islands | DFS |
| 130. Surrounded Regions | DFS / BFS |
| 994. Rotting Oranges | Multi-Source BFS |
| 417. Pacific Atlantic Water Flow | DFS / BFS |
| 827. Making A Large Island | DFS + Union Find |

---

# Interview Tips

- Use **DFS** or **BFS** to traverse connected land cells.
- Count the area while exploring the island.
- Update the maximum area after each DFS/BFS.
- Mark visited cells immediately after visiting them.
- Each land cell is visited exactly once, leading to an **O(m × n)** solution.
