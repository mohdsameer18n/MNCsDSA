# Climbing Stairs

## Problem Statement

You are climbing a staircase with `n` steps.

Each time, you can climb either:

- **1 step**
- **2 steps**

Return the number of distinct ways to reach the top.

---

# Approach 1: Recursion

## Idea

At every step, there are two choices:

- Take **1 step**
- Take **2 steps**

The total number of ways from the current step is the sum of the ways obtained by taking either choice.

### Recurrence

```
ways(i) = ways(i + 1) + ways(i + 2)
```

### Base Cases

- If `i == n`, one valid way is found → return `1`.
- If `i > n`, the path is invalid → return `0`.

---

## Java Code

```java
class Solution {

    public int solve(int i, int n) {

        if (i > n)
            return 0;

        if (i == n)
            return 1;

        int oneStep = solve(i + 1, n);
        int twoStep = solve(i + 2, n);

        return oneStep + twoStep;
    }

    public int climbStairs(int n) {
        return solve(0, n);
    }
}
```

### Time Complexity

- **O(2ⁿ)**

### Space Complexity

- **O(n)** (Recursion stack)

### Drawback

The same subproblems are solved repeatedly, leading to **Time Limit Exceeded (TLE)** for larger values of `n`.

---

# Approach 2: Memoization (Top-Down DP)

## Idea

Instead of solving the same subproblem multiple times, store the answer for each value of `n` in a DP array.

### DP State

```
dp[n]
```

Represents the number of ways to climb `n` stairs.

### Recurrence

```
dp[n] = dp[n - 1] + dp[n - 2]
```

### Base Cases

- `n <= 3`
  - `1 → 1`
  - `2 → 2`
  - `3 → 3`

---

## Java Code

```java
class Solution {

    public int solve(int n, int[] dp) {

        if (n <= 3)
            return n;

        if (dp[n] != -1)
            return dp[n];

        dp[n] = solve(n - 1, dp) + solve(n - 2, dp);

        return dp[n];
    }

    public int climbStairs(int n) {

        int[] dp = new int[n + 1];
        Arrays.fill(dp, -1);

        return solve(n, dp);
    }
}
```

### Time Complexity

- **O(n)**

### Space Complexity

- **O(n)** (DP array)
- **O(n)** (Recursion stack)

---

## Why Memoization?

Consider `n = 5`.

```
                solve(5)
               /       \
         solve(4)     solve(3)
         /     \         |
    solve(3) solve(2)   ...
```

The state `solve(3)` is computed multiple times.

Memoization stores its result after the first computation and reuses it whenever needed.

---

# Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| Recursion | **O(2ⁿ)** | **O(n)** |
| Memoization | **O(n)** | **O(n)** + **O(n)** recursion stack |

---

## Key Takeaways

- **Recursion** is simple but inefficient due to overlapping subproblems.
- **Memoization** stores previously computed results, reducing the time complexity from **O(2ⁿ)** to **O(n)**.
- This problem follows the Fibonacci recurrence:

```
ways(n) = ways(n - 1) + ways(n - 2)
```

making Dynamic Programming the optimal approach.
