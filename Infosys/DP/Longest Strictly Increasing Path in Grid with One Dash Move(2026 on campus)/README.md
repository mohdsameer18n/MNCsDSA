# Longest Strictly Increasing Path in Grid with One Dash Move

## Problem Statement

You are given an `N × M` grid of integers.

A path consists of a sequence of visited cells. The **length of the path** is the **number of cells visited**.

From any cell, you can move **one step** in any of the four directions:

- Up
- Down
- Left
- Right

You may move to a neighboring cell only if its value is **strictly greater** than the current cell.

Additionally, you are allowed to use **at most one Dash move** during the entire path.

A Dash move allows you to jump **exactly two cells** in one of the four directions.

For a Dash move:

- The destination cell must be inside the grid.
- The destination value must be strictly greater than the current value.
- The skipped cell is ignored completely and is **not counted** as a visited cell.

Return the **maximum number of cells** that can be visited.

---

## Example

### Input

```text
Grid =
[
  [1,100,2,3],
  [4,5,6,7],
  [8,9,10,11],
  [12,13,14,15]
]
```

### Output

```text
7
```

### Explanation

One of the longest paths is

```text
1 → 4 → 8 → 12 → 13 → 14 → 15
```

Number of visited cells = **7**

---

## Approach

We use **DFS + Memoization (Dynamic Programming).**

Since a Dash move can be used only once, each state depends on:

- Current row
- Current column
- Whether the Dash has already been used

State:

```text
dfs(row, col, dashUsed)
```

where

- `dashUsed = 0` → Dash not used yet
- `dashUsed = 1` → Dash already used

For every state:

1. Try all four normal moves.
2. If Dash is available, try all four Dash moves.
3. Store the answer in DP to avoid recomputation.

---

## DP State

```text
dp[row][col][2]
```

- `dp[row][col][0]` = longest path starting here without using Dash.
- `dp[row][col][1]` = longest path starting here after Dash has been used.

---

## Algorithm

1. Start DFS from every cell.
2. Explore all valid normal moves.
3. Explore Dash moves if not already used.
4. Store results using memoization.
5. Return the maximum answer.

---

## Time Complexity

```text
O(N × M)
```

Each state `(row, col, dashUsed)` is computed only once.

---

## Space Complexity

```text
O(N × M)
```

Used for memoization and recursion stack.

---

## Java Solution

```java
class Solution {

    int n, m;
    int[][] grid;
    int[][][] dp;

    int[] dr = {-1, 1, 0, 0};
    int[] dc = {0, 0, -1, 1};

    public int longestIncreasingPath(int[][] grid) {

        this.grid = grid;
        n = grid.length;
        m = grid[0].length;

        dp = new int[n][m][2];

        int ans = 0;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                ans = Math.max(ans, dfs(i, j, 0));
            }
        }

        return ans;
    }

    private int dfs(int r, int c, int dashUsed) {

        if (dp[r][c][dashUsed] != 0)
            return dp[r][c][dashUsed];

        int best = 1;

        // Normal Moves
        for (int k = 0; k < 4; k++) {

            int nr = r + dr[k];
            int nc = c + dc[k];

            if (isValid(nr, nc) && grid[nr][nc] > grid[r][c]) {
                best = Math.max(best, 1 + dfs(nr, nc, dashUsed));
            }
        }

        // Dash Move
        if (dashUsed == 0) {

            for (int k = 0; k < 4; k++) {

                int nr = r + 2 * dr[k];
                int nc = c + 2 * dc[k];

                if (isValid(nr, nc) && grid[nr][nc] > grid[r][c]) {
                    best = Math.max(best, 1 + dfs(nr, nc, 1));
                }
            }
        }

        return dp[r][c][dashUsed] = best;
    }

    private boolean isValid(int r, int c) {

        return r >= 0 && r < n && c >= 0 && c < m;
    }
}
```

---

## Key Concepts

- DFS
- Dynamic Programming
- Memoization
- Grid Traversal
- State DP
- Graph on Grid

---

## Difficulty

**Hard (8.5/10)**

This problem is an extension of **LeetCode 329 (Longest Increasing Path in a Matrix)** by introducing an additional **Dash state**, making it a 3D Dynamic Programming problem.
