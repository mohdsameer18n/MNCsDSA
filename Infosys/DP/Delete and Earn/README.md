# Delete and Earn (LeetCode 740)

## Problem Statement

You are given an integer array `nums`.

You can perform the following operation any number of times:

- Pick any number `x`.
- Earn `x` points.
- Delete **all occurrences** of `x`.
- After deleting `x`, every occurrence of `x-1` and `x+1` is also removed.

Return the **maximum points** you can earn.

---

## Intuition

The problem looks different, but it can be transformed into the **House Robber** problem.

Suppose

```text
nums = [2,2,3,3,3,4]
```

Instead of considering each element separately, count the frequency of every value.

| Value | Frequency |
|-------:|----------:|
|2|2|
|3|3|
|4|1|

Now create an array

```text
arr[value] = frequency
```

```
Index : 0 1 2 3 4
arr   : 0 0 2 3 1
```

If we choose value **3**

Points earned

```
3 × 3 = 9
```

Since choosing **3** removes **2** and **4**, we cannot choose adjacent values.

This is exactly the same rule as **House Robber**:

- Rob current house → skip adjacent house.
- Take current value → skip adjacent value.

---

# DP State

```
solve(n)
```

Maximum points obtainable using values from **0...n**.

---

# Choices

### 1. Take current value

Earn

```java
n * arr[n]
```

Since adjacent values cannot be chosen,

move to

```java
solve(n-2)
```

```
take = n * arr[n] + solve(n-2)
```

---

### 2. Skip current value

Ignore current value.

```
skip = solve(n-1)
```

---

# Recurrence

```java
dp[n] = Math.max(
    n * arr[n] + solve(n-2),
    solve(n-1)
);
```

---

# Base Case

```java
if(n < 0)
    return 0;
```

No values are left.

Maximum points = **0**

---

# Memoization

If already computed,

```java
if(dp[n] != -1)
    return dp[n];
```

Return the stored result.

---

# Dry Run

## Input

```text
nums = [2,2,3,3,3,4]
```

---

## Step 1

Create frequency array.

```
Index : 0 1 2 3 4
arr   : 0 0 2 3 1
```

---

## Step 2

Start recursion

```
solve(4)
```

### Take 4

```
4 × 1 + solve(2)

=4+solve(2)
```

### Skip 4

```
solve(3)
```

---

## solve(2)

Take

```
2 × 2 + solve(0)

=4+0

=4
```

Skip

```
solve(1)=0
```

Maximum

```
dp[2]=4
```

---

## solve(3)

Take

```
3 × 3 + solve(1)

=9+0

=9
```

Skip

```
solve(2)=4
```

Maximum

```
dp[3]=9
```

---

## solve(4)

Take

```
4 × 1 + solve(2)

=4+4

=8
```

Skip

```
solve(3)=9
```

Maximum

```
dp[4]=9
```

Answer

```
9
```

---

# Recursion Tree

```
solve(4)
│
├── Take
│      4 + solve(2)
│
│      solve(2)
│      ├── Take
│      │      4 + solve(0)
│      └── Skip
│             solve(1)
│
└── Skip
       solve(3)
       ├── Take
       │      9 + solve(1)
       └── Skip
              solve(2)
```

---

# Complexity Analysis

### Time Complexity

```
O(n + maxValue)
```

- O(n) for building the frequency array.
- O(maxValue) for DP.

---

### Space Complexity

```
O(maxValue)
```

Used for the frequency array and memoization array.

---

# Java Code

```java
class Solution {

    public int solve(int arr[], int n, int[] dp) {

        if (n < 0) {
            return 0;
        }

        if (dp[n] != -1) {
            return dp[n];
        }

        int take = n * arr[n] + solve(arr, n - 2, dp);
        int skip = solve(arr, n - 1, dp);

        return dp[n] = Math.max(take, skip);
    }

    public int deleteAndEarn(int[] nums) {

        int max = 0;

        for (int e : nums) {
            max = Math.max(max, e);
        }

        int[] arr = new int[max + 1];

        // Frequency array
        for (int e : nums) {
            arr[e]++;
        }

        int[] dp = new int[max + 1];
        Arrays.fill(dp, -1);

        return solve(arr, arr.length - 1, dp);
    }
}
```

---

# Pattern Recognition

| Problem | DP State | Transition |
|----------|----------|------------|
| House Robber | `dp[i]` = maximum money till house `i` | `max(nums[i] + dp[i-2], dp[i-1])` |
| Delete and Earn | `dp[i]` = maximum points till value `i` | `max(i × freq[i] + dp[i-2], dp[i-1])` |

> **Key Insight:** Convert the input into a **frequency array**, where each value behaves like a house. Once transformed, the problem becomes identical to **House Robber**.
