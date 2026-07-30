# Partition Equal Subset Sum

## Problem Statement

Given an integer array `nums`, determine whether it can be partitioned into **two subsets** such that the sum of elements in both subsets is equal.

Return:

- `true` if such a partition exists.
- `false` otherwise.

---

# Approach 1: Recursion

## Idea

If the array can be divided into two equal subsets, then one subset must have a sum equal to:

```
target = totalSum / 2
```

For every element, there are two choices:

1. **Include** the current element in the subset.
2. **Exclude** the current element.

If any combination forms the target sum, return `true`.

---

## Base Cases

### Target becomes 0

```java
if(target == 0)
    return true;
```

A valid subset has been found.

---

### No elements left

```java
if(i >= nums.length)
    return false;
```

No more elements are available to form the remaining target.

---

## Recurrence

```
check(i, target) =
    include OR exclude

include = check(i+1, target-nums[i])
exclude = check(i+1, target)
```

---

## Java Code

```java
class Solution {

    public boolean check(int i, int[] nums, int target) {

        if (target == 0)
            return true;

        if (i >= nums.length)
            return false;

        boolean include = false;

        if (nums[i] <= target)
            include = check(i + 1, nums, target - nums[i]);

        boolean exclude = check(i + 1, nums, target);

        return include || exclude;
    }

    public boolean canPartition(int[] nums) {

        int totalSum = 0;

        for (int num : nums)
            totalSum += num;

        if (totalSum % 2 != 0)
            return false;

        int target = totalSum / 2;

        return check(0, nums, target);
    }
}
```

---

### Time Complexity

- **O(2ⁿ)**

---

### Space Complexity

- **O(n)** (Recursion stack)

---

### Drawback

The same `(index, target)` states are computed multiple times, leading to **Time Limit Exceeded (TLE)** for larger inputs.

---

# Approach 2: Memoization (Top-Down DP)

## Idea

Store the result of every state `(index, target)`.

If the same state is encountered again, return the stored result instead of recomputing it.

---

## DP State

```
dp[i][target]
```

- `true` → Target can be formed.
- `false` → Target cannot be formed.
- `null` → State not computed yet.

---

## Recurrence

```
include = check(i+1, target-nums[i])

exclude = check(i+1, target)

dp[i][target] = include || exclude
```

---

## Algorithm

1. Calculate the total sum.
2. If the total sum is odd, return `false`.
3. Set `target = totalSum / 2`.
4. Create a DP table initialized with `null`.
5. Recursively try:
   - Include current element.
   - Exclude current element.
6. Store every computed result.

---

## Java Code

```java
class Solution {

    public boolean check(int i, int[] nums, int target, Boolean[][] dp) {

        if (target == 0)
            return true;

        if (i >= nums.length)
            return false;

        if (dp[i][target] != null)
            return dp[i][target];

        boolean include = false;

        if (nums[i] <= target)
            include = check(i + 1, nums, target - nums[i], dp);

        boolean exclude = check(i + 1, nums, target, dp);

        return dp[i][target] = include || exclude;
    }

    public boolean canPartition(int[] nums) {

        int totalSum = 0;

        for (int num : nums)
            totalSum += num;

        if (totalSum % 2 != 0)
            return false;

        int target = totalSum / 2;

        Boolean[][] dp = new Boolean[nums.length][target + 1];

        return check(0, nums, target, dp);
    }
}
```

---

## Example

### Input

```
nums = [1,5,11,5]
```

### Total Sum

```
1 + 5 + 11 + 5 = 22
```

Target:

```
22 / 2 = 11
```

Possible subset:

```
[11]
```

Remaining subset:

```
[1,5,5]
```

Both have sum **11**, so the answer is:

```
true
```

---

## Why Memoization?

Without memoization, identical states are solved repeatedly.

Example:

```
check(0,11)
      /      \
 include    exclude
    |          |
check(1,10) check(1,11)
      \       /
     check(2,5)
```

The state `check(2,5)` can be reached from multiple paths.

Memoization stores its answer after the first computation and reuses it whenever needed.

---

# Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| Recursion | **O(2ⁿ)** | **O(n)** |
| Memoization | **O(n × target)** | **O(n × target)** + **O(n)** |

where:

- `n` = number of elements
- `target = totalSum / 2`

---

# Key Takeaways

- The problem is reduced to finding a subset with sum `totalSum / 2`.
- If the total sum is odd, partitioning is impossible.
- Each element has two choices:
  - **Include** it in the subset.
  - **Exclude** it.
- Memoization stores results for each `(index, target)` pair, reducing the time complexity from **O(2ⁿ)** to **O(n × target)**.
- Sorting the array is **not required** for correctness in this DP solution; it can be omitted.
