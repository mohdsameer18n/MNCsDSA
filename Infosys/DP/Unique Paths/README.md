# Unique Paths

## Problem Statement

Given two integers `m` and `n`, representing the dimensions of an `m × n` grid, a robot starts at the top-left corner `(0,0)` and wants to reach the bottom-right corner `(m-1,n-1)`.

The robot can move only:
- Right
- Down

Return the total number of unique paths.

---

# Approach 1: Recursion

## Idea

From every cell, the robot has two possible choices:

- Move **Right**
- Move **Down**

The total number of paths from the current cell is:

```
paths(i, j) = paths(i, j + 1) + paths(i + 1, j)
```

### Base Cases

- If the destination is reached, return `1`.
- If the indices go outside the grid, return `0`.

---

## Algorithm

1. Start recursion from `(0,0)`.
2. Explore the right cell.
3. Explore the down cell.
4. Return the sum of both recursive calls.

---

## Java Code

```java
class Solution {

    public int solve(int i, int j, int m, int n) {

        if (i == m - 1 && j == n - 1)
            return 1;

        if (i >= m || j >= n)
            return 0;

        int right = solve(i, j + 1, m, n);
        int down = solve(i + 1, j, m, n);

        return right + down;
    }

    public int uniquePaths(int m, int n) {
        return solve(0, 0, m, n);
    }
}
```

### Time Complexity

- **O(2^(m+n))**

### Space Complexity

- **O(m+n)** (Recursion stack)

### Drawback

Many subproblems are solved repeatedly, causing **Time Limit Exceeded (TLE)** for larger grids.

---

# Approach 2: Memoization (Top-Down DP)

## Idea

The recursive solution recalculates the same states multiple times.

Store the answer for each state `(i, j)` in a DP table so that it is computed only once.

### DP State

```
dp[i][j]
```

Stores the number of unique paths from `(i,j)` to the destination.

---

## Transition

```
dp[i][j] =
    solve(i, j + 1)
  + solve(i + 1, j)
```

---

## Algorithm

1. Create a DP array initialized with `-1`.
2. Start recursion from `(0,0)`.
3. If the current state is already computed, return it.
4. Otherwise compute:
   - Right
   - Down
5. Store the answer in DP.
6. Return the stored value.

---

## Java Code

```java
class Solution {

    public int solve(int i, int j, int m, int n, int[][] dp) {

        if (i == m - 1 && j == n - 1)
            return 1;

        if (i >= m || j >= n)
            return 0;

        if (dp[i][j] != -1)
            return dp[i][j];

        int right = solve(i, j + 1, m, n, dp);
        int down = solve(i + 1, j, m, n, dp);

        return dp[i][j] = right + down;
    }

    public int uniquePaths(int m, int n) {

        int[][] dp = new int[m][n];

        for (int i = 0; i < m; i++) {
            Arrays.fill(dp[i], -1);
        }

        return solve(0, 0, m, n, dp);
    }
}
```

### Time Complexity

- **O(m × n)**

### Space Complexity

- **O(m × n)** (DP table)
- **O(m + n)** (Recursion stack)

---

## Why Memoization?

Consider the recursion tree:

```
               solve(0,0)
               /        \
         solve(0,1)   solve(1,0)
             \           /
             solve(1,1)
```

The state `solve(1,1)` is reached from multiple paths. In plain recursion, it is recomputed every time.

Memoization stores the result after the first computation and reuses it whenever the same state is encountered.

---

# Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| Recursion | **O(2^(m+n))** | **O(m+n)** |
| Memoization (Top-Down DP) | **O(m × n)** | **O(m × n)** + **O(m+n)** |

---

## Key Takeaways

- **Recursion** is simple and intuitive but inefficient due to repeated calculations.
- **Memoization** eliminates redundant work by storing intermediate results.
- Memoization reduces the time complexity from **exponential** to **polynomial**, making it suitable for large inputs.
