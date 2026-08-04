# Maximal Square (LeetCode 221)

## Problem

Given an `m × n` binary matrix filled with `'0'`s and `'1'`s, find the **largest square containing only `1`s** and return its **area**.

---

## Idea

For every cell `(i, j)`, find the **side length of the largest square** starting from that cell.

If the current cell is `'1'`, then the square can be extended only if:

- Down cell can form a square.
- Right cell can form a square.
- Diagonal cell can form a square.

The smallest among these three determines how large the current square can be.

---

## State

`dp[i][j]` = Side length of the largest square whose **top-left corner** is `(i, j)`.

---

## Recurrence

If the current cell is `'1'`:

```java
dp[i][j] = 1 + Math.min(down, Math.min(right, diagonal));
```

Otherwise:

```java
dp[i][j] = 0;
```

where

- `down = dp[i+1][j]`
- `right = dp[i][j+1]`
- `diagonal = dp[i+1][j+1]`

---

## Why do we use `Math.min()`?

The problem asks for the **maximum square**, but we use **minimum** because a square is only as large as its weakest side.

Example:

```
1 1
1 0
```

At `(0,0)`:

- Down = 1
- Right = 1
- Diagonal = 0

Using `max`:

```
1 + max(1,1,0) = 2 ❌
```

This incorrectly says a `2 × 2` square exists.

Using `min`:

```
1 + min(1,1,0) = 1 ✅
```

Correct, because the bottom-right corner is `0`.

A larger square can exist **only if all three neighboring squares are large enough**.

---

## Why `max = Math.max(max, dp[i][j])`?

`dp[i][j]` gives the largest square starting **only from one cell**.

We compare every cell and store the largest side length found.

```java
max = Math.max(max, dp[i][j]);
```

---

## Why return `max * max`?

`max` stores the **side length** of the largest square.

The problem asks for the **area**.

Example:

Largest square:

```
1 1
1 1
```

Side length = `2`

Area =

```
2 × 2 = 4
```

Hence,

```java
return max * max;
```

---

## Dry Run

Input:

```
1 1
1 1
```

### Step 1

```
solve(1,1)

down = 0
right = 0
diagonal = 0

dp[1][1] = 1
```

### Step 2

```
solve(1,0)

down = 0
right = 1
diagonal = 0

dp[1][0] = 1
```

### Step 3

```
solve(0,1)

down = 1
right = 0
diagonal = 0

dp[0][1] = 1
```

### Step 4

```
solve(0,0)

down = 1
right = 1
diagonal = 1

dp[0][0] = 2
```

Final DP:

```
2 1
1 1
```

Maximum side length:

```
max = 2
```

Answer:

```
Area = 2 × 2 = 4
```

---

## Complexity

### Time

```
O(m × n)
```

Each cell is computed only once because of memoization.

### Space

```
O(m × n)
```

For the DP table.

---

## Java Solution

```java
class Solution {
    int max = 0;

    public int solve(int i, int j, char[][] matrix, Integer[][] dp) {

        int m = matrix.length;
        int n = matrix[0].length;

        if (i >= m || j >= n)
            return 0;

        if (dp[i][j] != null)
            return dp[i][j];

        int down = solve(i + 1, j, matrix, dp);
        int right = solve(i, j + 1, matrix, dp);
        int diagonal = solve(i + 1, j + 1, matrix, dp);

        if (matrix[i][j] == '1') {
            dp[i][j] = 1 + Math.min(down, Math.min(right, diagonal));
            max = Math.max(max, dp[i][j]);
        } else {
            dp[i][j] = 0;
        }

        return dp[i][j];
    }

    public int maximalSquare(char[][] matrix) {

        int m = matrix.length;
        int n = matrix[0].length;

        Integer[][] dp = new Integer[m][n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                solve(i, j, matrix, dp);
            }
        }

        return max * max;
    }
}
```
