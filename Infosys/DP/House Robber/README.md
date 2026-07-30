# House Robber

## Problem Statement

You are a professional robber planning to rob houses along a street.

Each house contains a certain amount of money. The only constraint is that **adjacent houses cannot be robbed**, otherwise the police will be alerted.

Given an integer array `nums`, where `nums[i]` represents the amount of money in the `iᵗʰ` house, return the **maximum amount of money** you can rob without robbing two adjacent houses.

---

# Approach: Memoization (Top-Down DP)

## Idea

At every house, there are two choices:

1. **Rob the current house**
   - Add the current house's money.
   - Skip the next house.
   - Move to `i + 2`.

2. **Skip the current house**
   - Move to the next house (`i + 1`).

The answer is the maximum of these two choices.

---

## DP State

```
dp[i]
```

Represents the maximum money that can be robbed starting from house `i`.

---

## Recurrence

```
rob(i) = max(
            nums[i] + rob(i + 2),
            rob(i + 1)
         )
```

---

## Base Case

### No houses left

```java
if(i >= nums.length)
    return 0;
```

No money can be robbed.

---

## Algorithm

1. Start from the first house.
2. Compute:
   - Rob the current house.
   - Skip the current house.
3. Store the maximum result in the DP array.
4. Return the stored value.

---

## Java Code

```java
class Solution {

    public int solve(int[] nums, int i, int[] dp) {

        if (i >= nums.length)
            return 0;

        if (dp[i] != -1)
            return dp[i];

        int take = nums[i] + solve(nums, i + 2, dp);
        int leave = solve(nums, i + 1, dp);

        return dp[i] = Math.max(take, leave);
    }

    public int rob(int[] nums) {

        int[] dp = new int[nums.length + 1];
        Arrays.fill(dp, -1);

        return solve(nums, 0, dp);
    }
}
```

---

## Example

### Input

```
nums = [2,7,9,3,1]
```

Possible choices:

```
House:   2   7   9   3   1
Index:   0   1   2   3   4
```

Optimal robbery:

```
2 + 9 + 1 = 12
```

Maximum money:

```
12
```

---

## Why Memoization?

Without memoization, the same states are computed repeatedly.

Example:

```
                 rob(0)
               /        \
         rob(2)        rob(1)
         /    \        /    \
     rob(4) rob(3) rob(3) rob(2)
```

The states `rob(2)` and `rob(3)` are solved multiple times.

Memoization stores each computed state once and reuses it whenever needed.

---

## Complexity Analysis

### Time Complexity

- **O(n)**

Each index is computed only once.

### Space Complexity

- **O(n)** (DP array)
- **O(n)** (Recursion stack)

---

# Key Takeaways

- Each house has **two choices**:
  - **Take** the current house and skip the next.
  - **Leave** the current house and move to the next.
- Memoization stores the maximum profit for every index, avoiding repeated computations.
- The solution reduces the time complexity from **O(2ⁿ)** (recursive) to **O(n)**.
- This is a classic **1D Dynamic Programming** problem where each state depends on the next one or two states.
