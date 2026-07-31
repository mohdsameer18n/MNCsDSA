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

```text
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

### No elements left

```java
if(i >= nums.length)
    return false;
```

No more elements are available to form the remaining target.

---

## Recurrence

```text
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

## Time Complexity

- **O(2ⁿ)**

## Space Complexity

- **O(n)** (Recursion stack)

---

## Drawback

The same `(index, target)` states are computed repeatedly, causing **Time Limit Exceeded (TLE)** for larger inputs.

---

# Approach 2: Memoization (Top-Down DP)

## Idea

Store the result of every state `(index, target)`.

If the same state is encountered again, return the stored result instead of recomputing it.

---

## DP State

```text
dp[i][target]
```

- `true` → Target can be formed.
- `false` → Target cannot be formed.
- `null` → State has not been computed yet.

---

## Recurrence

```text
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
   - Include the current element.
   - Exclude the current element.
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

```text
nums = [1,5,11,5]
```

### Total Sum

```text
1 + 5 + 11 + 5 = 22
```

Target:

```text
22 / 2 = 11
```

Possible subset:

```text
[11]
```

Remaining subset:

```text
[1,5,5]
```

Both subsets have sum **11**, so the answer is:

```text
true
```

---

## Why Memoization?

Without memoization, identical states are solved repeatedly.

```text
check(0,11)
      /      \
 include    exclude
    |          |
check(1,10) check(1,11)
      \       /
     check(2,5)
```

The state `check(2,5)` can be reached from multiple paths.

Memoization stores the answer after the first computation and reuses it whenever needed.

---

# Approach 3: Tabulation (Bottom-Up DP)

## Idea

Instead of solving the problem recursively, build the solution from the **last index** towards the **first**.

Define:

```text
dp[i][t]
```

where:

- `i` = current index
- `t` = target sum

Meaning:

> `dp[i][t]` is `true` if we can form sum `t` using elements from index `i` to `n-1`.

The final answer is:

```text
dp[0][target]
```

---

## Base Case

When all elements have been processed (`i = n`):

- Sum `0` is always possible.
- Any positive sum is impossible.

Initialize:

```java
for (int i = 0; i <= n; i++) {
    dp[i][0] = true;
}
```

All remaining values stay `false` by default.

---

## Transition

For every state `(i, t)`:

### Include the current element

Only possible if:

```java
nums[i] <= t
```

Then:

```java
include = dp[i + 1][t - nums[i]];
```

### Exclude the current element

```java
exclude = dp[i + 1][t];
```

### Store the answer

```java
dp[i][t] = include || exclude;
```

---

## Algorithm

1. Compute the total sum.
2. If the total sum is odd, return `false`.
3. Set `target = totalSum / 2`.
4. Create a `(n + 1) × (target + 1)` DP table.
5. Initialize `dp[i][0] = true`.
6. Traverse indices from `n - 1` to `0`.
7. For every target from `0` to `target`:
   - Include the current element.
   - Exclude the current element.
   - Store the result.
8. Return `dp[0][target]`.

---

## Java Code

```java
class Solution {

    public boolean canPartition(int[] nums) {

        int n = nums.length;

        int totalSum = 0;
        for (int num : nums)
            totalSum += num;

        if (totalSum % 2 != 0)
            return false;

        int target = totalSum / 2;

        boolean[][] dp = new boolean[n + 1][target + 1];

        // Base Case
        for (int i = 0; i <= n; i++) {
            dp[i][0] = true;
        }

        // Fill the table
        for (int i = n - 1; i >= 0; i--) {
            for (int t = 0; t <= target; t++) {

                boolean include = false;

                if (nums[i] <= t)
                    include = dp[i + 1][t - nums[i]];

                boolean exclude = dp[i + 1][t];

                dp[i][t] = include || exclude;
            }
        }

        return dp[0][target];
    }
}
```

---

## DP State Visualization

For:

```text
nums = [1,5,11,5]
target = 11
```

| DP State | Meaning |
|----------|---------|
| `dp[4][0]` | `true` (empty subset forms sum 0) |
| `dp[4][1...11]` | `false` |
| `dp[3][*]` | Using only the last element |
| `dp[2][*]` | Using elements from index 2 onward |
| `dp[1][*]` | Using elements from index 1 onward |
| `dp[0][*]` | Using the entire array |

The required answer is stored in:

```text
dp[0][11]
```

---

## Why Fill the Table Backwards?

Each state depends on the next row:

```text
dp[i][t]
   │
   ├── dp[i+1][t]
   └── dp[i+1][t-nums[i]]
```

Since `dp[i]` depends on `dp[i+1]`, the table must be filled **from bottom to top**.

---

# Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| Recursion | **O(2ⁿ)** | **O(n)** |
| Memoization | **O(n × target)** | **O(n × target)** + **O(n)** |
| Tabulation | **O(n × target)** | **O(n × target)** |

where:

- `n` = Number of elements
- `target = totalSum / 2`

---

# Key Takeaways

- The problem reduces to finding a subset with sum `totalSum / 2`.
- If the total sum is odd, partitioning is impossible.
- **Recursion** explores every possible subset.
- **Memoization** stores intermediate results to avoid repeated computations.
- **Tabulation** builds the solution iteratively without recursion.
- Both Memoization and Tabulation solve the problem in **O(n × target)** time.
- Sorting the array is **not required** for any of the three approaches.
