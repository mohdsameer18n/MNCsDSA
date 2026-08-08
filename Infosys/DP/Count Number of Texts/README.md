# 2266 – Count Number of Texts

## Problem Statement

A phone keypad contains the following letters:

```text
2 → abc
3 → def
4 → ghi
5 → jkl
6 → mno
7 → pqrs
8 → tuv
9 → wxyz
```

A key can be pressed multiple times to select a character.

For example:

```text
2   → a
22  → b
222 → c
```

For keys `7` and `9`:

```text
7   → p
77  → q
777 → r
7777 → s

9   → w
99  → x
999 → y
9999 → z
```

Given a string `pressedKeys` representing the sequence of key presses, return the **number of possible text messages** that could have been typed.

Return the answer modulo:

```text
1,000,000,007
```

---

# Example 1

## Input

```text
pressedKeys = "22233"
```

## Output

```text
8
```

---

# Example 2

## Input

```text
pressedKeys = "2"
```

## Output

```text
1
```

Because:

```text
2 → a
```

---

# Example 3

## Input

```text
pressedKeys = "222"
```

## Output

```text
4
```

The possible partitions are:

```text
2 + 2 + 2
2 + 22
22 + 2
222
```

These represent:

```text
aaa
ab
ba
c
```

Therefore:

```text
Answer = 4
```

---

# Key Observation

We are **not generating the actual letters**.

Instead, we are counting how many ways the pressed keys can be divided into groups.

For example:

```text
222
```

can be divided into:

```text
2 | 2 | 2
2 | 22
22 | 2
222
```

So there are:

```text
4 ways
```

This makes the problem a **Dynamic Programming** problem.

---

# Important Rule

The digits in one group must be the **same**.

For example:

```text
223
```

We can take:

```text
2
22
```

but we cannot take:

```text
223
```

because:

```text
2 != 3
```

Therefore, when the current character changes, we stop.

---

# Maximum Group Size

Normal keys have 3 letters:

```text
2 → abc
3 → def
4 → ghi
5 → jkl
6 → mno
8 → tuv
```

Therefore:

```text
Maximum consecutive presses = 3
```

Keys `7` and `9` have 4 letters:

```text
7 → pqrs
9 → wxyz
```

Therefore:

```text
Maximum consecutive presses = 4
```

---

# Approach

We use:

```text
Recursion + Memoization
```

Define:

```text
solve(i)
```

as:

> Number of possible messages that can be created using the substring starting at index `i`.

At every index:

- Take 1 character.
- Take 2 characters if they are the same.
- Take 3 characters if they are the same.
- Take 4 characters for `7` and `9` if they are the same.

Add all possibilities.

---

# Base Case

```java
if (i == n) {
    return 1;
}
```

When we reach the end of the string, we have successfully created one valid message.

Therefore:

```text
solve(n) = 1
```

---

# Memoization

We use:

```java
Integer[] dp
```

where:

```text
dp[i] = number of ways from index i
```

If `dp[i]` has already been calculated:

```java
if (dp[i] != null) {
    return dp[i];
}
```

we simply return it.

This prevents repeated calculations.

---

# Recurrence

For keys:

```text
2, 3, 4, 5, 6, 8
```

we can have:

```text
dp[i] =
    dp[i + 1]
  + dp[i + 2]
  + dp[i + 3]
```

provided the consecutive characters are equal.

For keys:

```text
7 and 9
```

we can additionally have:

```text
dp[i + 4]
```

So:

```text
dp[i] =
    dp[i + 1]
  + dp[i + 2]
  + dp[i + 3]
  + dp[i + 4]
```

when valid.

---

# Dry Run

Consider:

```text
pressedKeys = "222"
```

Initially:

```text
solve(0)
```

---

## solve(0)

```text
i = 0
current character = '2'
limit = 3
```

Possible groups:

```text
2
22
222
```

Therefore:

```text
solve(0)
=
solve(1)
+
solve(2)
+
solve(3)
```

---

## solve(1)

Remaining:

```text
22
```

Possible groups:

```text
2
22
```

Therefore:

```text
solve(1)
=
solve(2)
+
solve(3)
```

---

## solve(2)

Remaining:

```text
2
```

Only one choice:

```text
2
```

Therefore:

```text
solve(2) = solve(3)
```

---

## solve(3)

We reached the end:

```text
i == n
```

Therefore:

```text
solve(3) = 1
```

---

# Calculate DP Values

From:

```text
solve(2)
```

we get:

```text
solve(2) = 1
```

Therefore:

```text
dp[2] = 1
```

From:

```text
solve(1)
```

we get:

```text
solve(1)
=
solve(2) + solve(3)

= 1 + 1

= 2
```

Therefore:

```text
dp[1] = 2
```

Finally:

```text
solve(0)
=
solve(1)
+
solve(2)
+
solve(3)

= 2 + 1 + 1

= 4
```

Therefore:

```text
dp[0] = 4
```

Final answer:

```text
4
```

---

# DP Table

For:

```text
pressedKeys = "222"
```

the DP table is:

```text
Index     Remaining     Ways
--------------------------------
0         222            4
1         22             2
2         2              1
3         ""             1
```

Therefore:

```text
dp = [4, 2, 1, 1]
```

---

# Recursion Tree

```text
                       222
                    /   |   \
                   2    22   222
                  / \    |
                 2  22   2
                 |   |   |
                 2   2  END
                 |
                END
```

The four valid partitions are:

```text
2 + 2 + 2
2 + 22
22 + 2
222
```

---

# Java Implementation

```java
import java.util.*;

class Solution {

    static final int MOD = 1000000007;

    public int solve(int i, String s, Integer[] dp) {

        int n = s.length();

        // Reached the end
        if (i == n) {
            return 1;
        }

        // Already calculated
        if (dp[i] != null) {
            return dp[i];
        }

        int ways = 0;

        char ch = s.charAt(i);

        // 7 and 9 have 4 possible letters
        int limit = (ch == '7' || ch == '9') ? 4 : 3;

        for (int j = i;
             j < n && j - i + 1 <= limit;
             j++) {

            // The characters must be the same
            if (s.charAt(j) != ch) {
                break;
            }

            ways = (ways + solve(j + 1, s, dp)) % MOD;
        }

        return dp[i] = ways;
    }

    public int countTexts(String pressedKeys) {

        Integer[] dp = new Integer[pressedKeys.length()];

        return solve(0, pressedKeys, dp);
    }
}
```

---

# Why `j - i + 1`?

This expression:

```java
j - i + 1
```

calculates how many characters we are currently taking.

Example:

```text
i = 0
j = 0

j - i + 1
= 0 - 0 + 1
= 1
```

We are taking:

```text
"2"
```

Next:

```text
i = 0
j = 1

1 - 0 + 1
= 2
```

We are taking:

```text
"22"
```

Next:

```text
i = 0
j = 2

2 - 0 + 1
= 3
```

We are taking:

```text
"222"
```

---

# Why `break`?

```java
if (s.charAt(j) != ch) {
    break;
}
```

Suppose:

```text
s = "223"
```

Starting from index `0`:

```text
ch = '2'
```

We can take:

```text
2
22
```

But when:

```text
j = 2
```

we have:

```text
s.charAt(2) = '3'
```

Since:

```text
3 != 2
```

we stop.

So:

```text
223
```

cannot be treated as one group.

---

# Why MOD?

The number of possible messages grows very quickly for long strings.

Therefore, the problem requires the answer modulo:

```text
1,000,000,007
```

We use:

```java
ways = (ways + solve(j + 1, s, dp)) % MOD;
```

This keeps the number manageable.

Use:

```java
static final int MOD = 1_000_000_007;
```

---

# Complexity

Let:

```text
n = length of pressedKeys
```

Each index is calculated only once because of memoization.

At each index, we check at most 4 characters.

### Time Complexity

```text
O(n)
```

### Space Complexity

```text
O(n)
```

The space is used by:

```text
DP array
+
recursion stack
```

---

# Pattern Recognition

This problem belongs to:

```text
1D DP
↓
String DP
↓
Partition DP
↓
Counting Ways
```

The key question is:

> **How many characters can I take from the current index?**

For each index:

```text
1 character
2 characters
3 characters
4 characters
```

depending on the keypad key.

---

# Similar LeetCode Problems

This problem is especially useful to learn these related problems:

### LeetCode 91 – Decode Ways

```text
String
↓
Take 1 or 2 characters
↓
Count ways
↓
DP
```

### LeetCode 1416 – Restore The Array

```text
String
↓
Partition into valid pieces
↓
Count ways
↓
DP
```

### LeetCode 639 – Decode Ways II

```text
String
↓
Multiple decoding choices
↓
Count ways
↓
DP + MOD
```

---

# Difference From LeetCode 17

Do not confuse these two keypad problems.

## LeetCode 17 – Letter Combinations

Input:

```text
23
```

Generate:

```text
ad
ae
af
bd
be
bf
cd
ce
cf
```

Pattern:

```text
Backtracking
```

---

## LeetCode 2266 – Count Number of Texts

Input:

```text
222
```

Count:

```text
2 + 2 + 2
2 + 22
22 + 2
222
```

Pattern:

```text
Dynamic Programming
+
Recursion
+
Memoization
```

---

# Key Takeaway

When you see:

```text
Consecutive keypad digits
+
Different numbers of repeated presses
+
Count possible messages
```

Think:

```text
1D DP + Memoization
```

At every index, ask:

```text
Can I take 1 character?
Can I take 2 characters?
Can I take 3 characters?
Can I take 4 characters?
```

Then recursively solve the remaining string.

**Core pattern:**

```text
Current Index
      ↓
Try 1 / 2 / 3 / 4 characters
      ↓
Solve remaining string
      ↓
Add the number of ways
      ↓
Memoize
```
