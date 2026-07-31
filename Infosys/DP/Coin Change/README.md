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

- ---

# Approach 2: Tabulation (Bottom-Up DP)

## Idea

Instead of solving the problem recursively, we build the solution iteratively using a DP table.

Define:

```text
dp[i][target]
```

where:

- `i` = current coin index
- `target` = remaining amount

Meaning:

> `dp[i][target]` represents the minimum number of coins required to make `target` using coins from index `i` to `n-1`.

The final answer is stored in:

```text
dp[0][amount]
```

---

## Base Case

When the target amount is `0`, no coins are required.

```java
for (int i = 0; i <= n; i++) {
    dp[i][0] = 0;
}
```

Initially, every other state is marked as impossible.

```java
for (int i = 0; i <= n; i++) {
    Arrays.fill(dp[i], Integer.MAX_VALUE);
}
```

---

## Transition

For every state `(i, target)`:

### Include the current coin

If the current coin can be used:

```java
include = dp[i][target - coins[i]];
```

Since a coin is taken:

```java
include++;
```

Notice that we stay at the **same index** because each coin can be used an unlimited number of times.

---

### Exclude the current coin

Skip the current coin and move to the next one.

```java
exclude = dp[i + 1][target];
```

---

### Store the Minimum

```java
dp[i][target] = Math.min(include, exclude);
```

---

## Algorithm

1. Create a DP table of size `(n + 1) × (amount + 1)`.
2. Initialize every value with `Integer.MAX_VALUE`.
3. Set `dp[i][0] = 0` for every row.
4. Traverse the coin indices from `n - 1` to `0`.
5. For every target from `1` to `amount`:
   - Compute the include choice.
   - Compute the exclude choice.
   - Store the minimum.
6. If `dp[0][amount]` is still `Integer.MAX_VALUE`, return `-1`; otherwise, return the answer.

---

## Java Code

```java
class Solution {

    public int coinChange(int[] coins, int amount) {

        int n = coins.length;

        int[][] dp = new int[n + 1][amount + 1];

        // Initialize all states as impossible
        for (int i = 0; i <= n; i++) {
            Arrays.fill(dp[i], Integer.MAX_VALUE);
        }

        // Base case: 0 coins needed to make amount 0
        for (int i = 0; i <= n; i++) {
            dp[i][0] = 0;
        }

        // Fill the DP table
        for (int i = n - 1; i >= 0; i--) {
            for (int target = 1; target <= amount; target++) {

                int include = Integer.MAX_VALUE;

                // Include current coin (reuse allowed)
                if (coins[i] <= target) {
                    include = dp[i][target - coins[i]];

                    if (include != Integer.MAX_VALUE) {
                        include++;
                    }
                }

                // Exclude current coin
                int exclude = dp[i + 1][target];

                dp[i][target] = Math.min(include, exclude);
            }
        }

        return dp[0][amount] == Integer.MAX_VALUE ? -1 : dp[0][amount];
    }
}
```

---

## DP State Visualization

For:

```text
coins = [1,2,5]
amount = 11
```

The DP table is filled from the bottom row upward.

| DP State | Meaning |
|----------|---------|
| `dp[3][0]` | 0 coins needed to make amount 0 |
| `dp[3][1...11]` | Impossible (∞) |
| `dp[2][*]` | Using only coin 5 |
| `dp[1][*]` | Using coins 2 and 5 |
| `dp[0][*]` | Using all coins |

The required answer is stored in:

```text
dp[0][11]
```

which equals **3**.

---

## Why Fill the Table Backwards?

Each state depends on:

```text
dp[i][target]
      │
      ├── dp[i][target - coins[i]]
      └── dp[i + 1][target]
```

- `dp[i][target - coins[i]]` represents **including** the current coin (stay at the same index because coins are reusable).
- `dp[i + 1][target]` represents **excluding** the current coin.

Since `dp[i]` depends on the row below (`dp[i + 1]`), we compute the table **from bottom to top**.

---

# Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| Memoization | **O(n × amount)** | **O(n × amount)** + **O(amount)** |
| Tabulation | **O(n × amount)** | **O(n × amount)** |

where:

- `n` = Number of coin denominations
- `amount` = Target amount

---

# Key Takeaways

- Every coin has two choices:
  - **Include** it (stay on the same index because coins can be reused).
  - **Exclude** it (move to the next coin).
- `Integer.MAX_VALUE` represents an impossible state.
- Always check that the include state is not `Integer.MAX_VALUE` before adding `1` to avoid integer overflow.
- Memoization solves the problem recursively by caching results.
- Tabulation builds the same solution iteratively, avoiding recursion while maintaining **O(n × amount)** time complexity.
