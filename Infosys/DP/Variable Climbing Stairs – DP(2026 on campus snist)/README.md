# Variable Climbing Stairs

## Problem Statement

You are climbing a staircase with `n` steps.

The allowed jumps depend on the **current index**:

- If the current index is **even**, you can climb either:
  - **1 step**
  - **2 steps**

- If the current index is **odd**, you can climb either:
  - **1 step**
  - **3 steps**

Return the number of **distinct ways** to reach the top.

---

# Approach 1: Recursion

## Idea

At every index, the available choices depend on whether the index is even or odd.

### Even Index

You can:

- Take **1 step**
- Take **2 steps**

### Odd Index

You can:

- Take **1 step**
- Take **3 steps**

The total number of ways is the sum of the ways obtained from each valid choice.

### Recurrence

For an even index:

```text
ways(i) = ways(i + 1) + ways(i + 2)

import java.util.*;

class Solution {

    public int solve(int i, int n, int[] dp) {

        if (i == n)
            return 1;

        if (i > n)
            return 0;

        if (dp[i] != -1)
            return dp[i];

        if (i % 2 == 0) {

            // Even index → 1 or 2 steps
            dp[i] = solve(i + 1, n, dp)
                  + solve(i + 2, n, dp);

        } else {

            // Odd index → 1 or 3 steps
            dp[i] = solve(i + 1, n, dp)
                  + solve(i + 3, n, dp);
        }

        return dp[i];
    }

    public int climbStairs(int n) {

        int[] dp = new int[n + 1];
        Arrays.fill(dp, -1);

        return solve(0, n, dp);
    }
}

Approach 2: Tabulation (Bottom-Up DP)

class Solution {

    public int climbStairs(int n) {

        int[] dp = new int[n + 1];

        dp[0] = 1;

        for (int i = 0; i <= n; i++) {

            // Take 1 step
            if (i + 1 <= n) {
                dp[i + 1] += dp[i];
            }

            // Even index → take 2 steps
            if (i % 2 == 0) {

                if (i + 2 <= n) {
                    dp[i + 2] += dp[i];
                }

            } else {

                // Odd index → take 3 steps
                if (i + 3 <= n) {
                    dp[i + 3] += dp[i];
                }
            }
        }

        return dp[n];
    }
}
