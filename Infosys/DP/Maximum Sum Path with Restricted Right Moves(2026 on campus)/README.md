# Maximum Sum Path with Restricted Right Moves

## Problem Statement

You are given a `row × col` matrix of integers.

You start at the **top-left** cell `(0,0)` and need to reach the **bottom-right** cell `(row-1,col-1)`.

### Allowed Moves

- **Down** → `(i+1, j)` (always allowed)
- **Right** → `(i, j+1)` **only if**
  ```
  matrix[i][j] < matrix[i][j+1]
  ```

## Objective

Find the **maximum possible sum** of the values along a valid path (including both the starting and ending cells).

If no valid path exists, return **-1**.

---

## Example

### Input

```text
3 3
1 2 3
2 5 6
4 7 8
```

### Valid Maximum Sum Path

```text
1
↓
2
→
5
↓
7
→
8
```

Sum

```text
1 + 2 + 5 + 7 + 8 = 23
```

### Output

```text
23
```

---

## Approach

Use **DFS + Memoization (Top-Down Dynamic Programming)**.

For every cell:

- Move **Down** (always possible if inside the matrix).
- Move **Right** only when
  ```
  current < right
  ```
- Store the best answer for every cell in a DP table to avoid recomputation.

If neither move can reach the destination, mark the cell as **invalid**.

---

## Algorithm

1. If the current cell is the destination, return its value.
2. If already computed, return the stored result.
3. Recursively compute:
   - Down move
   - Right move (only if allowed)
4. Take the maximum of both.
5. If no valid path exists, return a negative sentinel value.
6. Otherwise store
   ```
   matrix[i][j] + best
   ```
   in the DP table.

---

## Java Solution

```java
import java.util.*;

public class Main {

    static final int NEG = -1_000_000_000;
    static final int NOT_VISITED = Integer.MIN_VALUE;

    static int solve(int i, int j, int[][] mat, int[][] dp) {

        int n = mat.length;
        int m = mat[0].length;

        if (i == n - 1 && j == m - 1)
            return mat[i][j];

        if (dp[i][j] != NOT_VISITED)
            return dp[i][j];

        int down = NEG;
        int right = NEG;

        if (i + 1 < n)
            down = solve(i + 1, j, mat, dp);

        if (j + 1 < m && mat[i][j] < mat[i][j + 1])
            right = solve(i, j + 1, mat, dp);

        int best = Math.max(down, right);

        if (best == NEG)
            return dp[i][j] = NEG;

        return dp[i][j] = mat[i][j] + best;
    }

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();
        int m = scan.nextInt();

        int[][] mat = new int[n][m];

        for (int i = 0; i < n; i++)
            for (int j = 0; j < m; j++)
                mat[i][j] = scan.nextInt();

        int[][] dp = new int[n][m];

        for (int i = 0; i < n; i++)
            Arrays.fill(dp[i], NOT_VISITED);

        int ans = solve(0, 0, mat, dp);

        if (ans == NEG)
            System.out.println(-1);
        else
            System.out.println(ans);
    }
}
```

---

## Dry Run

Input

```text
1 2 3
2 5 6
4 7 8
```

```
solve(0,0)

Down → solve(1,0)
Right → solve(0,1)

Maximum path found:

1
↓
2
→
5
↓
7
→
8

Sum = 23
```

---

## Complexity Analysis

- **Time Complexity:** `O(row × col)`
- **Space Complexity:** `O(row × col)` (DP table)
- **Recursion Stack:** `O(row + col)`

---

## Key Concepts

- Dynamic Programming
- Memoization
- Depth First Search (DFS)
- Matrix Traversal
- Recursion
