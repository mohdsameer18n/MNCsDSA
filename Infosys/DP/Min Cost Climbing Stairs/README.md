# Min Cost Climbing Stairs (LeetCode 746)

## Problem Statement

You are given an integer array `cost`, where `cost[i]` is the cost of stepping on the `i-th` stair.

You can climb either:

- 1 step
- 2 steps

You can start from **index 0** or **index 1**.

Return the **minimum cost** required to reach the top.

---

# Intuition

At every stair, we have two choices:

- Climb **1 step**
- Climb **2 steps**

Since we need the **minimum total cost**, we choose the cheaper path.

---

# DP State

```
solve(i)
```

Represents the **minimum cost required to reach the top starting from stair `i`**.

---

# Choices

## Take 1 Step

```
cost[i] + solve(i+1)
```

---

## Take 2 Steps

```
cost[i] + solve(i+2)
```

Since we need the minimum cost,

```
solve(i) =
cost[i] + min(
    solve(i+1),
    solve(i+2)
)
```

---

# Recurrence

```java
dp[i] = cost[i] + Math.min(
            solve(i+1),
            solve(i+2)
        );
```

---

# Base Case

When we reach or cross the top,

```java
if(i >= cost.length)
    return 0;
```

No additional cost is required.

---

# Why Return

```java
Math.min(
    solve(0),
    solve(1)
)
```

The problem states that we can start from

- Stair **0**
- Stair **1**

So we compute both possibilities and return the smaller cost.

---

# Dry Run

## Input

```text
cost = [10,15,20]
```

### solve(2)

```
20 + min(0,0)

=20
```

---

### solve(1)

```
15 + min(20,0)

=15
```

---

### solve(0)

```
10 + min(15,20)

=25
```

---

Final Answer

```
min(25,15)

=15
```

---

# Recursion Tree

```
solve(0)
│
├── solve(1)
│   │
│   ├── solve(2)
│   │   ├── solve(3)=0
│   │   └── solve(4)=0
│   │
│   └── solve(3)=0
│
└── solve(2)
    ├── solve(3)=0
    └── solve(4)=0
```

---

# Memoization

If already computed,

```java
if(dp[i] != null)
    return dp[i];
```

This avoids solving the same subproblem multiple times.

---

# Complexity Analysis

### Time Complexity

```
O(n)
```

Each index is computed only once.

### Space Complexity

```
O(n)
```

- DP array
- Recursive stack

---

# Java Code

```java
class Solution {

    public int solve(int i, int[] cost, Integer[] dp) {

        if(i >= cost.length){
            return 0;
        }

        if(dp[i] != null){
            return dp[i];
        }

        int one = solve(i + 1, cost, dp);
        int two = solve(i + 2, cost, dp);

        return dp[i] = cost[i] + Math.min(one, two);
    }

    public int minCostClimbingStairs(int[] cost) {

        Integer[] dp = new Integer[cost.length];

        return Math.min(
                solve(0, cost, dp),
                solve(1, cost, dp)
        );
    }
}
```

---

# Pattern Recognition

| Problem | DP State | Transition |
|----------|----------|------------|
| Climbing Stairs | Number of ways from `i` | `dp[i] = dp[i+1] + dp[i+2]` |
| Min Cost Climbing Stairs | Minimum cost from `i` | `dp[i] = cost[i] + min(dp[i+1], dp[i+2])` |

## Key Difference

- **Climbing Stairs:** Count all possible ways → use `+`.
- **Min Cost Climbing Stairs:** Choose the cheaper path → use `min()`.
