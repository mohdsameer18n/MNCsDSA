# Best Time to Buy and Sell Stock II

**LeetCode:** [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

## Problem Description

You are given an integer array `prices` where:

```text
prices[i] = price of the stock on day i
```

You can make **unlimited transactions**.

For each transaction:

```text
Buy → Sell
```

You cannot hold more than one stock at a time.

Return the **maximum profit** you can achieve.

---

## Example 1

```text
Input:
prices = [7,1,5,3,6,4]

Output:
7
```

Transactions:

```text
Buy at 1 → Sell at 5
Profit = 4

Buy at 3 → Sell at 6
Profit = 3

Total = 4 + 3 = 7
```

---

# DP Approach

We maintain two states.

### 1. `hold`

`hold` represents the maximum profit we can have if we are **currently holding a stock**.

Initially:

```java
int hold = -prices[0];
```

Why negative?

If we buy the stock on day `0`:

```text
profit = -prices[0]
```

For example:

```text
prices[0] = 7

hold = -7
```

---

### 2. `cash`

`cash` represents the maximum profit when we are **not holding a stock**.

Initially:

```java
int cash = 0;
```

Because before doing anything:

```text
profit = 0
```

---

# State Transitions

## Holding a Stock

There are two choices:

### Option 1: Continue holding

```text
hold
```

### Option 2: Buy today

We must use the money available in `cash`:

```text
cash - prices[i]
```

Therefore:

```java
hold = Math.max(hold, cash - prices[i]);
```

---

## Not Holding a Stock

There are two choices:

### Option 1: Continue without stock

```text
cash
```

### Option 2: Sell the stock today

If we were holding the stock before today:

```text
hold + prices[i]
```

Therefore:

```java
cash = Math.max(cash, hold + prices[i]);
```

However, we must use the **previous `hold` value**, because the current day's `hold` should not be used for the same transaction.

---

# Correct Java Code

```java
class Solution {
    public int maxProfit(int[] prices) {

        int hold = -prices[0];
        int cash = 0;

        for (int i = 1; i < prices.length; i++) {

            int prevHold = hold;
            int prevCash = cash;

            // Buy or continue holding
            hold = Math.max(prevHold, prevCash - prices[i]);

            // Sell or continue without stock
            cash = Math.max(prevCash, prevHold + prices[i]);
        }

        return cash;
    }
}
```

---

# Why `prevHold` Is Necessary

Consider this:

```java
hold = Math.max(hold, cash - prices[i]);

cash = Math.max(cash, hold + prices[i]);
```

The problem is that `hold` has already been updated.

So `cash` may use the **new `hold` from the same day**.

Instead, save the old values:

```java
int prevHold = hold;
int prevCash = cash;
```

Then calculate both states from the previous day's states:

```java
hold = Math.max(prevHold, prevCash - prices[i]);

cash = Math.max(prevCash, prevHold + prices[i]);
```

This is the proper **2-state DP** transition.

---

# Dry Run

### Input

```text
prices = [7,1,5,3,6,4]
```

Initially:

```text
hold = -7
cash = 0
```

### Day 1 — Price = 1

```text
prevHold = -7
prevCash = 0
```

Calculate:

```text
hold = max(-7, 0 - 1)
     = -1

cash = max(0, -7 + 1)
     = 0
```

Result:

```text
hold = -1
cash = 0
```

---

### Day 2 — Price = 5

```text
prevHold = -1
prevCash = 0
```

```text
hold = max(-1, 0 - 5)
     = -1

cash = max(0, -1 + 5)
     = 4
```

Result:

```text
hold = -1
cash = 4
```

We effectively:

```text
Buy at 1
Sell at 5

Profit = 4
```

---

### Day 3 — Price = 3

```text
prevHold = -1
prevCash = 4
```

```text
hold = max(-1, 4 - 3)
     = 1

cash = max(4, -1 + 3)
     = 4
```

Result:

```text
hold = 1
cash = 4
```

We can use the previous profit of `4` to buy at `3`.

---

### Day 4 — Price = 6

```text
prevHold = 1
prevCash = 4
```

```text
hold = max(1, 4 - 6)
     = 1

cash = max(4, 1 + 6)
     = 7
```

Result:

```text
hold = 1
cash = 7
```

We sell:

```text
3 → 6

Profit = 3
```

Total:

```text
4 + 3 = 7
```

---

### Day 5 — Price = 4

```text
prevHold = 1
prevCash = 7
```

```text
hold = max(1, 7 - 4)
     = 3

cash = max(7, 1 + 4)
     = 7
```

Final:

```text
cash = 7
```

Therefore:

```text
Output = 7
```

---

# DP Table

| Day | Price | `hold` | `cash` |
| --: | ----: | -----: | -----: |
|   0 |     7 |     -7 |      0 |
|   1 |     1 |     -1 |      0 |
|   2 |     5 |     -1 |      4 |
|   3 |     3 |      1 |      4 |
|   4 |     6 |      1 |      7 |
|   5 |     4 |      3 |      7 |

Final answer:

```text
7
```

---

# State Diagram

```text
                 Buy
        ┌────────────────────┐
        ↓                    │
      CASH ───────────────→ HOLD
        ↑                    │
        │                    │ Sell
        └────────────────────┘
```

Transitions:

```text
CASH → HOLD
cash - price

HOLD → CASH
hold + price
```

---

# Complexity Analysis

Let `n` be the number of days.

### Time Complexity

We process each price exactly once:

```text
O(n)
```

### Space Complexity

Only four variables are used:

```text
O(1)
```

---

# Key Takeaway

This problem has two DP states:

```text
hold = maximum profit while holding stock
cash = maximum profit while not holding stock
```

The transitions are:

```java
hold = Math.max(prevHold, prevCash - price);

cash = Math.max(prevCash, prevHold + price);
```

Remember:

```text
BUY  → cash - price
SELL → hold + price
```

This **2-state DP pattern** is very important because it can be extended to harder stock problems such as transactions with cooldowns, transaction fees, or a limited number of transactions.
