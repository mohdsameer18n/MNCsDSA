# Minimum Path Sum (LeetCode 64) - Memoization (Top-Down DP)

## Problem Statement

You are given an `m × n` grid filled with non-negative numbers.

You start from the **top-left** cell `(0,0)` and need to reach the **bottom-right** cell `(m-1,n-1)`.

You can only move:

- Right →
- Down ↓

Return the **minimum path sum**.

---

# Intuition

At every cell `(i,j)`, we have only two choices:

- Move Down `(i+1, j)`
- Move Right `(i, j+1)`

Since we need the **minimum cost**, we choose the cheaper path.

---

# DP State

```
solve(i, j)
```

Represents:

> Minimum path sum from cell `(i,j)` to the destination.

---

# Recurrence

```
solve(i,j) =
grid[i][j] +
min(
    solve(i+1,j),
    solve(i,j+1)
)
```

---

# Base Cases

### 1. Destination Reached

```java
if(i == m-1 && j == n-1)
    return grid[i][j];
```

We reached the destination.

Return its value.

---

### 2. Outside Grid

```java
if(i >= m || j >= n)
    return Integer.MAX_VALUE;
```

This is an invalid path.

Returning `Integer.MAX_VALUE` ensures that `Math.min()` never chooses this path.

Example:

```
min(7, Integer.MAX_VALUE)

=7
```

---

# Memoization

Create

```java
Integer[][] dp = new Integer[m][n];
```

Initially,

```
null null null
null null null
null null null
```

If already computed,

```java
if(dp[i][j] != null)
    return dp[i][j];
```

This avoids solving the same state multiple times.

---

# Recursion Tree

Input

```
1 3 1
1 5 1
4 2 1
```

```
solve(0,0)
│
├── Down
│   solve(1,0)
│   │
│   ├── Down
│   │   solve(2,0)
│   │
│   └── Right
│       solve(1,1)
│
└── Right
    solve(0,1)
    │
    ├── Down
    │   solve(1,1)
    │
    └── Right
        solve(0,2)
```

Notice

```
solve(1,1)
```

is computed twice.

Memoization stores it once.

---

# Dry Run

Input

```
1 3 1
1 5 1
4 2 1
```

### solve(2,2)

```
1
```

---

### solve(2,1)

```
2 + min(INF,1)

=3
```

---

### solve(2,0)

```
4 + min(INF,3)

=7
```

---

### solve(1,2)

```
1 + min(1,INF)

=2
```

---

### solve(1,1)

```
5 + min(3,2)

=7
```

---

### solve(1,0)

```
1 + min(7,7)

=8
```

---

### solve(0,2)

```
1 + min(2,INF)

=3
```

---

### solve(0,1)

```
3 + min(7,3)

=6
```

---

### solve(0,0)

```
1 + min(8,6)

=7
```

Answer

```
7
```

---

# DP Table After Memoization

```
7 6 3
8 7 2
7 3 1
```

---

# Java Code

```java
class Solution {

    public int solve(int i, int j, int[][] grid, Integer[][] dp) {

        int m = grid.length;
        int n = grid[0].length;

        if (i == m - 1 && j == n - 1) {
            return grid[i][j];
        }

        if (i >= m || j >= n) {
            return Integer.MAX_VALUE;
        }

        if (dp[i][j] != null) {
            return dp[i][j];
        }

        int down = solve(i + 1, j, grid, dp);
        int right = solve(i, j + 1, grid, dp);

        int ans = Math.min(down, right);

        if (ans == Integer.MAX_VALUE) {
            return dp[i][j] = Integer.MAX_VALUE;
        }

        return dp[i][j] = grid[i][j] + ans;
    }

    public int minPathSum(int[][] grid) {

        int m = grid.length;
        int n = grid[0].length;

        Integer[][] dp = new Integer[m][n];

        return solve(0, 0, grid, dp);
    }
}
```

---

# Complexity Analysis

### Time Complexity

```
O(m × n)
```

Each cell is computed only once.

### Space Complexity

```
O(m × n)
```

- DP table
- Recursive stack

---

# Pattern Recognition

| Problem | DP State | Transition |
|----------|----------|------------|
| Unique Paths | Number of paths from `(i,j)` | `down + right` |
| Minimum Path Sum | Minimum cost from `(i,j)` | `grid[i][j] + min(down, right)` |

---

# Key Takeaways

- State: `solve(i, j)` = minimum cost from `(i,j)` to destination.
- Choices: Move **Right** or **Down**.
- Since it's a **minimum** problem, use `Math.min()`.
- Return `Integer.MAX_VALUE` for invalid paths so they are never chosen.
- Use `Integer[][] dp` and check `dp[i][j] != null` to avoid recomputation.
- Final answer is `solve(0,0)`.
```
