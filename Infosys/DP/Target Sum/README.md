# Target Sum (LeetCode 494)

## Problem Statement

You are given an integer array `nums` and an integer `target`.

You need to assign either a `'+'` or `'-'` sign before every element in the array such that the resulting expression equals `target`.

Return the **number of different expressions** that evaluate to the target.

---

## Example 1

### Input

```text
nums = [1,1,1,1,1]
target = 3
```

### Output

```text
5
```

### Explanation

There are five different ways:

```text
-1 +1 +1 +1 +1 = 3
+1 -1 +1 +1 +1 = 3
+1 +1 -1 +1 +1 = 3
+1 +1 +1 -1 +1 = 3
+1 +1 +1 +1 -1 = 3
```

---

## Example 2

### Input

```text
nums = [1]
target = 1
```

### Output

```text
1
```

---

# Intuition

Each number has two choices:

- Add (`+`)
- Subtract (`-`)

Brute Force explores all possibilities.

```text
              1
           /     \
         +1      -1
       /   \    /   \
     +1   -1 +1   -1
```

Time Complexity:

```text
O(2^n)
```

This is too slow.

---

# DP Optimization

Suppose

- Positive numbers sum = **P**
- Negative numbers sum = **N**

Then

```text
P - N = target
```

Also,

```text
P + N = totalSum
```

Adding both equations,

```text
2P = totalSum + target
```

Therefore,

```text
P = (totalSum + target) / 2
```

Now the problem becomes:

> Count the number of subsets whose sum equals **P**.

This converts the problem into **Count of Subsets with Given Sum**.

---

## Conditions

If

```text
totalSum < |target|
```

or

```text
(totalSum + target) is odd
```

Answer is

```text
0
```

because no valid partition exists.

---

# DP State

```text
dp[j]
```

represents

```text
Number of subsets having sum = j
```

---

# Transition

For every number,

```text
dp[j] += dp[j - num]
```

This means:

- Existing subsets remain.
- Add current number to all subsets having sum `j-num`.

---

# Algorithm

1. Calculate total sum.
2. Check invalid conditions.
3. Find required subset sum.

```text
subset = (totalSum + target) / 2
```

4. Initialize DP array.
5. Perform 0/1 Knapsack.
6. Return `dp[subset]`.

---

# Dry Run

### Input

```text
nums = [1,1,1,1,1]
target = 3
```

Total Sum

```text
5
```

Required subset

```text
(5 + 3) / 2 = 4
```

Now count subsets having sum **4**.

---

### DP Initialization

```text
dp = [1,0,0,0,0]
```

`dp[0]=1`

There is one way to make sum zero.

---

### First Number = 1

```text
dp = [1,1,0,0,0]
```

---

### Second Number = 1

```text
dp = [1,2,1,0,0]
```

---

### Third Number = 1

```text
dp = [1,3,3,1,0]
```

---

### Fourth Number = 1

```text
dp = [1,4,6,4,1]
```

---

### Fifth Number = 1

```text
dp = [1,5,10,10,5]
```

Answer

```text
dp[4] = 5
```

---

# Java Code

```java
class Solution {

    public int findTargetSumWays(int[] nums, int target) {

        int totalSum = 0;

        for (int num : nums)
            totalSum += num;

        if (Math.abs(target) > totalSum)
            return 0;

        if ((totalSum + target) % 2 != 0)
            return 0;

        int subset = (totalSum + target) / 2;

        int[] dp = new int[subset + 1];

        dp[0] = 1;

        for (int num : nums) {

            for (int j = subset; j >= num; j--) {

                dp[j] += dp[j - num];
            }
        }

        return dp[subset];
    }
}
```

---

# Complexity Analysis

### Time Complexity

```text
O(n × subset)
```

where

```text
subset = (totalSum + target) / 2
```

---

### Space Complexity

```text
O(subset)
```

---

# Pattern Recognition

This problem is a variation of:

- ✅ 0/1 Knapsack
- ✅ Subset Sum
- ✅ Count of Subsets with Given Sum
- ✅ Dynamic Programming

---

# Key Formula

```text
P - N = target
P + N = totalSum

⇒ 2P = totalSum + target

⇒ P = (totalSum + target) / 2
```

---

# Similar Problems

- LeetCode 416 – Partition Equal Subset Sum
- LeetCode 494 – Target Sum
- Count of Subsets with Given Sum
- 0/1 Knapsack
- Partition With Given Difference
- Minimum Subset Sum Difference

---

# Key Takeaways

- Every element has two choices: `+` or `-`.
- Convert the problem into **Count of Subsets with Given Sum**.
- Use **0/1 Knapsack DP** to count the subsets.
- This optimization reduces the complexity from **O(2ⁿ)** to **O(n × subset)**.
