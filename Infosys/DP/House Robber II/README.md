# House Robber II (LeetCode 213)

## Problem Statement

You are given an integer array `nums` where each element represents the amount of money in a house.

The houses are arranged in a **circle**, meaning the **first and last houses are adjacent**.

You cannot rob **two adjacent houses**.

Return the **maximum amount of money** you can rob without alerting the police.

---

## Example

### Input

```text
nums = [2,3,2]
```

### Output

```text
3
```

### Explanation

- Robbing house `0` and house `2` is not allowed because they are adjacent in a circle.
- The maximum money that can be robbed is `3`.

---

## Observation

For the linear House Robber problem, we use the recurrence:

```text
dp[i] = max(
    nums[i] + dp[i+2],
    dp[i+1]
)
```

However, this does **not** work directly for a circular arrangement because:

- First house and last house are adjacent.
- We cannot rob both.

---

## Key Idea

Break the circular problem into **two linear problems**.

### Case 1

Exclude the last house.

```text
[0 ........ n-2]
```

### Case 2

Exclude the first house.

```text
[1 ........ n-1]
```

The answer is:

```text
max(case1, case2)
```

---

## Recursive Relation

For every index:

```text
take = nums[i] + solve(i + 2)

skip = solve(i + 1)

dp[i] = max(take, skip)
```

---

## Memoization Algorithm

```
If n == 1
    return nums[0]

case1 = solve(0, n-2)

case2 = solve(1, n-1)

return max(case1, case2)
```

---

## Java Solution (Memoization)

```java
class Solution {

    public int solve(int i, int end, int[] nums, int[] dp) {

        if (i > end)
            return 0;

        if (dp[i] != -1)
            return dp[i];

        int take = nums[i] + solve(i + 2, end, nums, dp);
        int skip = solve(i + 1, end, nums, dp);

        return dp[i] = Math.max(take, skip);
    }

    public int rob(int[] nums) {

        int n = nums.length;

        if (n == 1)
            return nums[0];

        int[] dp1 = new int[n];
        int[] dp2 = new int[n];

        Arrays.fill(dp1, -1);
        Arrays.fill(dp2, -1);

        int case1 = solve(0, n - 2, nums, dp1);
        int case2 = solve(1, n - 1, nums, dp2);

        return Math.max(case1, case2);
    }
}
```

---

## Dry Run

### Input

```text
nums = [1,2,3,1]
```

### Case 1 (Exclude Last)

```text
Array:

1 2 3

solve(0)

take = 1 + solve(2)
skip = solve(1)

Result = 4
```

---

### Case 2 (Exclude First)

```text
Array:

2 3 1

solve(1)

take = 2 + 1 = 3
skip = 3

Result = 3
```

---

### Final Answer

```text
max(4,3)=4
```

---

## Complexity Analysis

### Time Complexity

```text
O(n)
```

Each index is computed only once in each DP run.

---

### Space Complexity

```text
O(n)
```

- DP array
- Recursive stack

---

## Pattern

Whenever a DP problem contains:

- Circular array
- First and last elements are adjacent
- Cannot select both first and last

Use:

```text
Answer =
max(
    solve(0, n-2),
    solve(1, n-1)
)
```

This converts the circular DP into two independent linear DP problems.

---

## Similar Problems

- House Robber I (LeetCode 198)
- House Robber II (LeetCode 213)
- Pizza With 3n Slices (LeetCode 1388)
- Sticker Collection Problems
- Circular Maximum Sum with Constraints
