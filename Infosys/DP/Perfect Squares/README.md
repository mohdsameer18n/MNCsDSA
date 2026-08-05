# Perfect Squares (LeetCode 279)

## Problem Statement

Given an integer `n`, return the **least number of perfect square numbers** whose sum equals `n`.

A **perfect square** is an integer that is the square of another integer.

Examples:
- `1 = 1²`
- `4 = 2²`
- `9 = 3²`
- `16 = 4²`

---

## Example 1

### Input

```text
n = 12
```

### Output

```text
3
```

### Explanation

```text
12 = 4 + 4 + 4
```

---

## Example 2

### Input

```text
n = 13
```

### Output

```text
2
```

### Explanation

```text
13 = 4 + 9
```

---

# Approach (Tabulation)

We use **Dynamic Programming (Bottom-Up)**.

Let

```text
dp[i] = Minimum number of perfect squares required to make sum i.
```

### Base Case

```text
dp[0] = 0
```

If the sum is `0`, no squares are required.

---

## Transition

For every number `i` from `1` to `n`, try every perfect square less than or equal to `i`.

Formula:

```text
dp[i] = min(dp[i], 1 + dp[i - square])
```

where

```text
square = 1, 4, 9, 16, ...
```

The `+1` represents choosing the current perfect square.

---

## Algorithm

1. Create a DP array of size `n + 1`.
2. Initialize all values to `Integer.MAX_VALUE`.
3. Set `dp[0] = 0`.
4. Iterate from `1` to `n`.
5. For every perfect square less than or equal to the current number:
   - Update the DP value.
6. Return `dp[n]`.

---

## Dry Run

### Input

```text
n = 12
```

Initially

```text
dp = [0,∞,∞,∞,∞,∞,∞,∞,∞,∞,∞,∞,∞]
```

### i = 1

Square:

```text
1
```

```text
dp[1] = 1 + dp[0]
      = 1
```

---

### i = 2

```text
dp[2] = 1 + dp[1]
      = 2
```

---

### i = 3

```text
dp[3] = 1 + dp[2]
      = 3
```

---

### i = 4

Squares:

```text
1
4
```

Using `1`

```text
1 + dp[3] = 4
```

Using `4`

```text
1 + dp[0] = 1
```

Minimum

```text
dp[4] = 1
```

---

### i = 8

Squares:

```text
1
4
```

```text
1 + dp[7] = 5

1 + dp[4] = 2
```

Minimum

```text
dp[8] = 2
```

---

### i = 9

Squares:

```text
1
4
9
```

```text
1 + dp[8] = 3

1 + dp[5] = 3

1 + dp[0] = 1
```

Minimum

```text
dp[9] = 1
```

---

### i = 12

Squares:

```text
1
4
9
```

```text
1 + dp[11] = 4

1 + dp[8] = 3

1 + dp[3] = 4
```

Minimum

```text
dp[12] = 3
```

Answer:

```text
3
```

---

## DP Table

| i |0|1|2|3|4|5|6|7|8|9|10|11|12|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| dp |0|1|2|3|1|2|3|4|2|1|2|3|3|

---

# Java Code

```java
import java.util.*;

class Solution {

    public int numSquares(int n) {

        int[] dp = new int[n + 1];

        Arrays.fill(dp, Integer.MAX_VALUE);

        dp[0] = 0;

        for (int i = 1; i <= n; i++) {

            for (int j = 1; j * j <= i; j++) {

                int square = j * j;

                dp[i] = Math.min(dp[i], 1 + dp[i - square]);
            }
        }

        return dp[n];
    }
}
```

---

## Complexity Analysis

### Time Complexity

```text
O(n × √n)
```

For every number from `1` to `n`, we try all perfect squares up to `√n`.

### Space Complexity

```text
O(n)
```

Used for the DP array.

---

## Key Concepts

- Dynamic Programming
- Tabulation (Bottom-Up DP)
- Unbounded Knapsack Pattern
- Minimum Cost / Minimum Steps DP

---

## Pattern Recognition

Whenever the recurrence is:

```text
f(n) = 1 + min(f(n - choice))
```

the tabulation template is:

```java
dp[0] = 0;

for (int i = 1; i <= n; i++) {
    for (each valid choice) {
        dp[i] = Math.min(dp[i], 1 + dp[i - choice]);
    }
}
```

This same pattern is used in:

- LeetCode 279 – Perfect Squares
- LeetCode 322 – Coin Change
- Minimum Cost DP Problems
- Minimum Steps Problems
- Unbounded Knapsack Variants
```
