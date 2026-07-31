# Minimum Cost For Tickets

## Problem Statement

You are given an array `days`, where `days[i]` represents the days you will travel, and an array `costs` where:

- `costs[0]` = cost of a **1-day** pass
- `costs[1]` = cost of a **7-day** pass
- `costs[2]` = cost of a **30-day** pass

A pass allows unlimited travel for its duration.

Return the **minimum cost** required to cover all travel days.

---

# Approach 1: Recursion

## Idea

For every travel day, we have three choices:

1. Buy a **1-day** pass.
2. Buy a **7-day** pass.
3. Buy a **30-day** pass.

After purchasing a pass, skip all travel days covered by that pass and recursively solve for the remaining days.

The answer is the minimum of the three choices.

---

## Base Case

When all travel days have been covered:

```java
if(i >= days.length)
    return 0;
```

No additional cost is needed.

---

## Recurrence

```text
solve(i) = min(
    cost1 + solve(next index after 1-day pass),
    cost7 + solve(next index after 7-day pass),
    cost30 + solve(next index after 30-day pass)
)
```

---

## Java Code

```java
class Solution {

    public int solve(int i, int[] days, int[] costs) {

        if (i >= days.length)
            return 0;

        int oneDay = costs[0] + solve(i + 1, days, costs);

        int j = i;
        while (j < days.length && days[j] < days[i] + 7)
            j++;

        int sevenDay = costs[1] + solve(j, days, costs);

        j = i;
        while (j < days.length && days[j] < days[i] + 30)
            j++;

        int thirtyDay = costs[2] + solve(j, days, costs);

        return Math.min(oneDay, Math.min(sevenDay, thirtyDay));
    }

    public int mincostTickets(int[] days, int[] costs) {
        return solve(0, days, costs);
    }
}
```

---

## Time Complexity

- **Exponential**

---

## Space Complexity

- **O(n)** (Recursion stack)

---

## Drawback

Many indices are solved repeatedly, resulting in **Time Limit Exceeded (TLE)**.

---

# Approach 2: Memoization (Top-Down DP)

## Idea

Store the minimum cost for every starting travel day.

If the same index is visited again, return the stored answer.

---

## DP State

```text
dp[i]
```

Represents the minimum cost required to cover travel days starting from index `i`.

---

## Algorithm

1. If all travel days are covered, return `0`.
2. If the state is already computed, return it.
3. Compute the cost for:
   - 1-day pass
   - 7-day pass
   - 30-day pass
4. Store the minimum answer.

---

## Java Code

```java
class Solution {

    Integer[] dp;

    public int solve(int i, int[] days, int[] costs) {

        if (i >= days.length)
            return 0;

        if (dp[i] != null)
            return dp[i];

        int oneDay = costs[0] + solve(i + 1, days, costs);

        int j = i;
        while (j < days.length && days[j] < days[i] + 7)
            j++;

        int sevenDay = costs[1] + solve(j, days, costs);

        j = i;
        while (j < days.length && days[j] < days[i] + 30)
            j++;

        int thirtyDay = costs[2] + solve(j, days, costs);

        return dp[i] = Math.min(oneDay, Math.min(sevenDay, thirtyDay));
    }

    public int mincostTickets(int[] days, int[] costs) {

        dp = new Integer[days.length];

        return solve(0, days, costs);
    }
}
```

---

# Approach 3: Tabulation (Bottom-Up DP)

## Idea

Instead of recursion, build the answer from the last travel day toward the first.

Define:

```text
dp[i]
```

where:

> `dp[i]` is the minimum cost required to cover travel days starting from index `i`.

The answer will be stored in:

```text
dp[0]
```

---

## Base Case

```java
dp[n] = 0;
```

If all travel days are covered, no additional cost is required.

---

## Transition

For every index:

```text
dp[i] = min(
    cost1 + dp[next1],
    cost7 + dp[next7],
    cost30 + dp[next30]
)
```

where:

- `next1 = i + 1`
- `next7` = first index not covered by the 7-day pass
- `next30` = first index not covered by the 30-day pass

---

## Algorithm

1. Create a DP array of size `n + 1`.
2. Set `dp[n] = 0`.
3. Traverse from `n-1` to `0`.
4. Find the next uncovered index for each pass.
5. Store the minimum cost.
6. Return `dp[0]`.

---

## Java Code

```java
class Solution {

    public int mincostTickets(int[] days, int[] costs) {

        int n = days.length;

        int[] dp = new int[n + 1];

        dp[n] = 0;

        for (int i = n - 1; i >= 0; i--) {

            int oneDay = costs[0] + dp[i + 1];

            int j = i;
            while (j < n && days[j] < days[i] + 7)
                j++;

            int sevenDay = costs[1] + dp[j];

            j = i;
            while (j < n && days[j] < days[i] + 30)
                j++;

            int thirtyDay = costs[2] + dp[j];

            dp[i] = Math.min(oneDay, Math.min(sevenDay, thirtyDay));
        }

        return dp[0];
    }
}
```

---

## Example

### Input

```text
days = [1,4,6,7,8,20]
costs = [2,7,15]
```

Possible strategy:

- Buy a **7-day pass** on day 1 → Covers days 1,4,6,7
- Buy a **1-day pass** on day 8
- Buy a **1-day pass** on day 20

Total cost:

```text
7 + 2 + 2 = 11
```

---

# Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| Recursion | Exponential | **O(n)** |
| Memoization | **O(n²)** | **O(n)** |
| Tabulation | **O(n²)** | **O(n)** |

---

# Key Takeaways

- At every travel day, there are **three choices**:
  - Buy a **1-day** pass.
  - Buy a **7-day** pass.
  - Buy a **30-day** pass.
- Purchasing a pass covers multiple future travel days, so we skip all covered days before solving the remaining problem.
- The DP state is based only on the **current travel day index**.
- **Memoization** stores the minimum cost for each index and avoids repeated computations.
- **Tabulation** computes the same states iteratively from the last travel day to the first.
- Both Memoization and Tabulation produce the optimal answer while avoiding the exponential complexity of plain recursion.
