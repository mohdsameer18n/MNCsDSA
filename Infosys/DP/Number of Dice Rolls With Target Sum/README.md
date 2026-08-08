# LeetCode 1155 – Number of Dice Rolls With Target Sum

## Problem Statement

You have `n` dice.

Each die has `k` faces numbered from:

```text
1 → k
```

Return the number of possible ways to roll the dice so that the sum of all dice is exactly `target`.

Since the answer can be very large, return it modulo:

```text
1,000,000,007
```

---

## Example 1

### Input

```text
n = 2
k = 6
target = 7
```

### Output

```text
6
```

### Explanation

We need two dice whose sum is `7`.

Possible combinations:

```text
1 + 6
2 + 5
3 + 4
4 + 3
5 + 2
6 + 1
```

Therefore:

```text
Answer = 6
```

---

# Example 2

### Input

```text
n = 1
k = 6
target = 3
```

### Output

```text
1
```

Only:

```text
3
```

can produce the target.

---

# Example 3

### Input

```text
n = 2
k = 3
target = 4
```

Possible combinations:

```text
1 + 3
2 + 2
3 + 1
```

Therefore:

```text
Output = 3
```

---

# Approach

We use:

```text
Recursion + Memoization
```

This is a **2D Dynamic Programming** problem.

Define:

```text
solve(dice, target)
```

as:

> Number of ways to get `target` using exactly `dice` remaining dice.

At every step, we try every possible face:

```text
1, 2, 3, ..., k
```

After choosing a face:

```text
dice decreases by 1
target decreases by face
```

So:

```text
solve(dice, target)
```

calls:

```text
solve(dice - 1, target - 1)
solve(dice - 1, target - 2)
...
solve(dice - 1, target - k)
```

---

# Base Cases

## Case 1: Successful

```java
if (dice == 0 && target == 0)
    return 1;
```

This means:

```text
All dice are used
AND
target is exactly reached
```

Therefore, we found **one valid combination**.

---

## Case 2: Impossible

```java
if (dice == 0 || target < 0)
    return 0;
```

### No dice left but target remains

Example:

```text
dice = 0
target = 3
```

Impossible.

Return:

```text
0
```

### Target becomes negative

Example:

```text
target = 2
face = 5
```

Then:

```text
target - face = -3
```

Impossible.

Return:

```text
0
```

---

# DP State

We use:

```java
Integer[][] dp
```

where:

```text
dp[dice][target]
```

stores:

> Number of ways to achieve `target` using `dice` dice.

For example:

```text
dp[2][7]
```

means:

> Number of ways to get sum `7` using `2` dice.

---

# Memoization

Before calculating a state:

```java
if (dp[dice][target] != null)
    return dp[dice][target];
```

If the state has already been calculated, return the stored answer.

This prevents calculating the same state multiple times.

---

# Recurrence

For every possible face:

```java
for (int face = 1; face <= k; face++)
```

we calculate:

```text
solve(dice - 1, target - face)
```

Therefore:

```text
dp[dice][target]
=
dp[dice-1][target-1]
+
dp[dice-1][target-2]
+
...
+
dp[dice-1][target-k]
```

Only valid states contribute to the answer.

---

# Full Dry Run

Consider:

```text
n = 2
k = 3
target = 4
```

We want:

```text
solve(2, 4)
```

---

## Step 1

We have:

```text
dice = 2
target = 4
```

Try all faces:

```text
face = 1
face = 2
face = 3
```

### Choose face = 1

Remaining:

```text
dice = 1
target = 3
```

Call:

```text
solve(1, 3)
```

For one die, possible faces are:

```text
1 → remaining target 2
2 → remaining target 1
3 → remaining target 0
```

Only:

```text
face = 3
```

reaches:

```text
solve(0,0)
```

which returns:

```text
1
```

Therefore:

```text
solve(1,3) = 1
```

This represents:

```text
1 + 3
```

---

### Choose face = 2

Remaining:

```text
dice = 1
target = 2
```

Call:

```text
solve(1,2)
```

Only:

```text
2
```

can reach the target.

Therefore:

```text
solve(1,2) = 1
```

This represents:

```text
2 + 2
```

---

### Choose face = 3

Remaining:

```text
dice = 1
target = 1
```

Only:

```text
1
```

can reach the target.

Therefore:

```text
solve(1,1) = 1
```

This represents:

```text
3 + 1
```

---

## Final Calculation

Therefore:

```text
solve(2,4)
=
solve(1,3)
+
solve(1,2)
+
solve(1,1)

= 1 + 1 + 1

= 3
```

Answer:

```text
3
```

---

# Recursion Tree

For:

```text
n = 2
k = 3
target = 4
```

the recursion looks like:

```text
                         (2,4)
                       /   |   \
                     1     2     3
                    /      |      \
                  (1,3)  (1,2)  (1,1)
                  / | \    / | \   / | \
                 1  2  3  1  2  3 1  2  3
                    |  |      |       |
                   ... 3      2       1
                       ↓      ↓       ↓
                      (0,0)  (0,0)  (0,0)
                         1       1       1
```

The successful paths are:

```text
1 → 3
2 → 2
3 → 1
```

Therefore:

```text
Answer = 3
```

---

# Why `dice - 1`?

When we choose a face, we have used one die.

For example:

```text
2 dice
```

Choose:

```text
face = 2
```

Now:

```text
remaining dice = 1
```

Therefore:

```java
solve(dice - 1, ...)
```

---

# Why `target - face`?

Suppose:

```text
target = 7
```

and the current die shows:

```text
face = 3
```

Then we still need:

```text
7 - 3 = 4
```

So:

```java
solve(dice - 1, target - face, ...)
```

becomes:

```java
solve(1, 4, ...)
```

---

# Why `MOD = 1000000007`?

The number of possible dice combinations can become extremely large.

For example, with many dice, the number of combinations can grow exponentially.

Therefore, the problem asks us to return:

```text
answer % 1,000,000,007
```

We calculate:

```java
ways = (ways + solve(...)) % MOD;
```

to keep the number manageable.

---

# Java Code

```java
class Solution {

    int MOD = 1000000007;

    public int solve(int dice, int target, int k, Integer[][] dp) {

        // All dice used and target reached
        if (dice == 0 && target == 0)
            return 1;

        // Impossible case
        if (dice == 0 || target < 0)
            return 0;

        // Already calculated
        if (dp[dice][target] != null)
            return dp[dice][target];

        int ways = 0;

        // Try every possible face
        for (int face = 1; face <= k; face++) {

            ways = (ways +
                    solve(dice - 1,
                          target - face,
                          k,
                          dp)) % MOD;
        }

        return dp[dice][target] = ways;
    }

    public int numRollsToTarget(int n, int k, int target) {

        Integer[][] dp = new Integer[n + 1][target + 1];

        return solve(n, target, k, dp);
    }
}
```

---

# Complexity Analysis

Let:

```text
n = number of dice
target = target sum
k = number of faces
```

There are approximately:

```text
n × target
```

different DP states.

For every state, we try:

```text
k
```

possible faces.

### Time Complexity

```text
O(n × target × k)
```

### Space Complexity

```text
O(n × target)
```

for the DP table.

The recursion stack requires:

```text
O(n)
```

additional space.

---

# Pattern Recognition

This problem is a classic:

```text
2D DP
+
Recursion
+
Memoization
+
Counting Ways
```

When you see:

```text
Choose something
+
Reduce a target
+
Count number of ways
```

think:

```text
DP
```

---

# Similar Problems

Useful problems to practice after this:

1. **LeetCode 2266 – Count Number of Texts**
2. **LeetCode 377 – Combination Sum IV**
3. **LeetCode 518 – Coin Change II**
4. **LeetCode 494 – Target Sum**
5. **LeetCode 91 – Decode Ways**
6. **LeetCode 1155 – Number of Dice Rolls With Target Sum**

---

# Key Takeaway

The entire solution can be remembered as:

```text
solve(dice, target)

        ↓

Try every face

        ↓

solve(dice - 1, target - face)

        ↓

Add all valid ways

        ↓

Store in dp[dice][target]
```

The most important recurrence is:

```text
ways =
    solve(dice-1, target-1)
  + solve(dice-1, target-2)
  + ...
  + solve(dice-1, target-k)
```

This is the core **2D DP counting pattern**.
