# Unique Paths II

## Problem Statement

A robot is located at the **top-left corner** of an `m × n` grid.

The robot can move only:

- Right `(→)`
- Down `(↓)`

Some cells contain obstacles (`1`), while free cells are represented by (`0`).

Return the **number of unique paths** from the top-left corner to the bottom-right corner.

If no path exists, return `0`.

---

## Example

### Input

```text
obstacleGrid =
[
 [0,0,0],
 [0,1,0],
 [0,0,0]
]
```

### Output

```text
2
```

### Explanation

Possible paths:

```text
Path 1:
→ → ↓ ↓

Path 2:
↓ ↓ → →
```

The robot cannot pass through the obstacle.

---

# Approach

This problem is a variation of **Unique Paths**.

Instead of counting all paths, we must **avoid obstacle cells**.

We use **Dynamic Programming (Memoization)**.

---

# DP State

Let

```text
dp[i][j]
```

represent the number of unique paths from cell `(i,j)` to the destination.

---

# Base Cases

### 1. Out of Bounds

If

```text
i >= m OR j >= n
```

Return

```text
0
```

because the robot cannot leave the grid.

---

### 2. Obstacle

If

```text
grid[i][j] == 1
```

Return

```text
0
```

because the robot cannot stand on an obstacle.

---

### 3. Destination

If

```text
(i == m-1 && j == n-1)
```

Return

```text
1
```

because we found one valid path.

---

### 4. Memoization

If

```text
dp[i][j] != -1
```

Return the stored value.

---

# Recurrence Relation

From every cell we have two choices.

Move Right

```text
(i, j+1)
```

Move Down

```text
(i+1, j)
```

Therefore,

```text
dp[i][j] =
solve(i, j+1)
+
solve(i+1, j)
```

---

# Algorithm

```
solve(i,j)

If outside grid
    return 0

If obstacle
    return 0

If destination
    return 1

If already computed
    return dp[i][j]

right = solve(i,j+1)

down = solve(i+1,j)

dp[i][j] = right + down

return dp[i][j]
```

Main Function

```
Initialize dp with -1

Return solve(0,0)
```

---

# Dry Run

Input

```text
0 0 0
0 1 0
0 0 0
```

Start

```text
solve(0,0)
```

It explores

```text
Right
Down
```

Obstacle at

```text
(1,1)
```

returns

```text
0
```

Destination

```text
(2,2)
```

returns

```text
1
```

Finally,

```text
dp[0][0] = 2
```

Answer

```text
2
```

---

# Recursion Tree

```text
solve(0,0)
├── solve(0,1)
│   ├── solve(0,2)
│   │   ├── solve(0,3)=0
│   │   └── solve(1,2)=1
│   └── solve(1,1)=0 (Obstacle)
└── solve(1,0)
    ├── solve(1,1)=0 (Obstacle)
    └── solve(2,0)
        ├── solve(2,1)=1
        │   ├── solve(2,2)=1
        │   └── solve(3,1)=0
        └── solve(3,0)=0
```

---

# Java Code (Memoization)

```java
class Solution {

    public int solve(int i, int j, int m, int n, int[][] grid, int[][] dp) {

        // Out of bounds
        if (i >= m || j >= n)
            return 0;

        // Obstacle
        if (grid[i][j] == 1)
            return 0;

        // Destination
        if (i == m - 1 && j == n - 1)
            return 1;

        // Memoization
        if (dp[i][j] != -1)
            return dp[i][j];

        int right = solve(i, j + 1, m, n, grid, dp);
        int down = solve(i + 1, j, m, n, grid, dp);

        return dp[i][j] = right + down;
    }

    public int uniquePathsWithObstacles(int[][] obstacleGrid) {

        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;

        int[][] dp = new int[m][n];

        for (int i = 0; i < m; i++) {
            Arrays.fill(dp[i], -1);
        }

        return solve(0, 0, m, n, obstacleGrid, dp);
    }
}
```

---

# Complexity Analysis

### Time Complexity

```text
O(m × n)
```

Each cell is computed only once.

---

### Space Complexity

```text
O(m × n)
```

- DP table: `O(m × n)`
- Recursive stack: `O(m + n)` in the worst case.

---

# DP Pattern

This is a **Grid DP** problem.

General recurrence:

```text
dp[i][j] =
solve(i, j+1)
+
solve(i+1, j)
```

with additional checks for:

- Out of bounds
- Obstacles
- Destination

---

# Similar Problems

- Unique Paths (LeetCode 62)
- Unique Paths II (LeetCode 63)
- Minimum Path Sum (LeetCode 64)
- Cherry Pickup (LeetCode 741)
- Dungeon Game (LeetCode 174)
- Triangle (LeetCode 120)
