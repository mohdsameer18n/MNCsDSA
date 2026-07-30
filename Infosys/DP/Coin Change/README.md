# Coin Change (Memoization / Top-Down DP)

## Problem Statement

You are given an integer array `coins` representing different coin denominations and an integer `amount`.

Return the **minimum number of coins** needed to make up the given amount.

- If it is impossible to make the amount, return **-1**.
- You may use each coin **unlimited times**.

---

## Approach: Memoization (Top-Down DP)

### Idea

For every coin, we have two choices:

1. **Include** the current coin
   - Reduce the remaining amount.
   - Stay on the same coin since it can be reused.

2. **Exclude** the current coin
   - Move to the next coin.

The answer is the minimum of these two choices.

---

## DP State

```
dp[i][amount]
```

Represents the minimum number of coins required to make the remaining `amount` using coins starting from index `i`.

---

## Recurrence

```
include = 1 + solve(i, amount - coins[i])
exclude = solve(i + 1, amount)

dp[i][amount] = min(include, exclude)
```

---

## Base Cases

### Amount becomes 0

```
if(amount == 0)
    return 0;
```

No more coins are needed.

---

### No coins left

```
if(i >= coins.length)
    return Integer.MAX_VALUE;
```

It is impossible to form the remaining amount.

---

## Algorithm

1. Start from the first coin.
2. Try including the current coin.
3. Try excluding the current coin.
4. Store the minimum answer in the DP table.
5. Return `-1` if no valid solution exists.

---

## Java Code

```java
class Solution {

    Integer[][] dp;

    public int coinChange(int[] coins, int amount) {

        dp = new Integer[coins.length][amount + 1];

        int ans = findMin(0, coins, amount, dp);

        return (ans == Integer.MAX_VALUE) ? -1 : ans;
    }

    public int findMin(int i, int[] coins, int amount, Integer[][] dp) {

        if (amount == 0)
            return 0;

        if (i >= coins.length)
            return Integer.MAX_VALUE;

        if (dp[i][amount] != null)
            return dp[i][amount];

        int include = Integer.MAX_VALUE;

        if (coins[i] <= amount) {
            include = findMin(i, coins, amount - coins[i], dp);

            if (include != Integer.MAX_VALUE)
                include++;
        }

        int exclude = findMin(i + 1, coins, amount, dp);

        return dp[i][amount] = Math.min(include, exclude);
    }
}
```

---

## Example

### Input

```
coins = [1,2,5]
amount = 11
```

### Choices

```
11
│
├── Take 1
│      ↓
│     10
│
├── Take 2
│      ↓
│      9
│
└── Take 5
       ↓
       6
```

The optimal solution is:

```
5 + 5 + 1 = 11
```

Minimum coins = **3**

---

## Why Memoization?

Without memoization, the same states are solved repeatedly.

For example:

```
solve(0,11)
      /      \
include     exclude
   |
solve(0,10)
   |
solve(0,9)
```

The same `(index, amount)` combinations can be reached through different paths.

Memoization stores each computed state once and reuses it whenever needed.

---

## Complexity Analysis

### Time Complexity

- **O(n × amount)**

where:

- `n` = number of coin denominations
- `amount` = target amount

Each state `(i, amount)` is computed only once.

---

### Space Complexity

- **O(n × amount)** (DP table)
- **O(amount)** recursion stack in the worst case

---

# Key Takeaways

- Every coin has **two choices**:
  - Include it (reuse the same coin).
  - Exclude it (move to the next coin).
- Memoization avoids solving identical subproblems multiple times.
- `Integer.MAX_VALUE` is used to represent an impossible state and must be checked before adding `1` to avoid integer overflow.
- The solution efficiently finds the **minimum number of coins** needed to form the target amount.
