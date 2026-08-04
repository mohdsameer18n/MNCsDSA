# Distinct Subsequences (LeetCode 115)

## Problem

Given two strings:

- `s` (source string)
- `t` (target string)

Return the **number of distinct subsequences** of `s` that are equal to `t`.

A subsequence is formed by deleting zero or more characters without changing the order of the remaining characters.

---

## Example

### Input

```text
s = "rabbbit"
t = "rabbit"
```

### Output

```text
3
```

### Explanation

There are **3** different ways to delete one `'b'` from `"rabbbit"` to obtain `"rabbit"`.

---

# DP Idea

At every state `(i, j)`:

- `i` → current index in `s`
- `j` → current index in `t`

We want to know:

> **How many ways can `s[i...]` form `t[j...]`?**

---

# DP State

```
dp[i][j]
```

represents

> Number of distinct subsequences of `s[i...]` equal to `t[j...]`.

---

# Base Cases

## Case 1 : Target is completely matched

```java
if(j == n)
    return 1;
```

Example

```
s = "abc"
t = ""
```

One valid subsequence exists:

```
Delete all characters.
```

Return

```
1
```

---

## Case 2 : Source is exhausted

```java
if(i == m)
    return 0;
```

Example

```
s = ""
t = "abc"
```

Impossible to form `"abc"`.

Return

```
0
```

> **Important:** Check `j == n` before `i == m`.  
> If both strings finish together, we have successfully formed one subsequence.

---

# Transition

## Case 1 : Characters Match

```
s.charAt(i) == t.charAt(j)
```

Example

```
s : rabbbit
    ^
t : rabbit
    ^
```

We have **two choices**.

### Choice 1 : Take the character

Use this matching character.

```java
take = solve(i+1, j+1);
```

---

### Choice 2 : Skip the character

Ignore the current character of `s`.

```java
skip = solve(i+1, j);
```

Total ways

```java
dp[i][j] = take + skip;
```

---

## Case 2 : Characters Don't Match

```
s.charAt(i) != t.charAt(j)
```

Current character cannot be used.

Skip it.

```java
dp[i][j] = solve(i+1, j);
```

---

# Recurrence

```java
if(s.charAt(i) == t.charAt(j))
{
    dp[i][j] =
        solve(i+1, j+1) +
        solve(i+1, j);
}
else
{
    dp[i][j] =
        solve(i+1, j);
}
```

---

# Dry Run

## Input

```
s = "rabbbit"
t = "rabbit"
```

Indexes

```
s : r a b b b i t
    0 1 2 3 4 5 6

t : r a b b i t
    0 1 2 3 4 5
```

---

## Step 1

```
solve(0,0)
```

Characters

```
r == r
```

Two choices

```
Take

solve(1,1)

Skip

solve(1,0)
```

Skip branch cannot match `'r'` anymore.

```
Return 0
```

Continue with

```
solve(1,1)
```

---

## Step 2

```
a == a
```

Again

```
Take

solve(2,2)

Skip

solve(2,1)
```

Skip branch returns

```
0
```

Continue

```
solve(2,2)
```

---

## Step 3

Characters

```
b == b
```

Two choices

```
Take

solve(3,3)

Skip

solve(3,2)
```

---

### solve(3,3)

Characters

```
b == b
```

Again

```
Take

solve(4,4)

Skip

solve(4,3)
```

Eventually

```
solve(4,4)=1

solve(4,3)=1
```

Therefore

```
solve(3,3)

=1+1

=2
```

---

### solve(3,2)

Characters

```
b == b
```

Again

```
Take

solve(4,3)=1

Skip

solve(4,2)=0
```

Therefore

```
solve(3,2)

=1+0

=1
```

---

## Step 4

Back to

```
solve(2,2)
```

Now

```
Take = 2

Skip = 1
```

Hence

```
dp[2][2]

=2+1

=3
```

---

## Final

```
solve(1,1)=3

solve(0,0)=3
```

Answer

```
3
```

---

# Recursion Tree

```
solve(0,0)
      |
      | r==r
      |
      +----------------+
      |                |
    Take             Skip(0)
      |
solve(1,1)
      |
      | a==a
      |
      +----------------+
      |                |
    Take             Skip(0)
      |
solve(2,2)
      |
      | b==b
      |
      +----------------+
      |                |
    Take(2)         Skip(1)
      |
      3
```

---

# Why Answer = 3?

There are **three ways** to choose two `'b'` characters from the three consecutive `'b'`s.

```
r a b(2) b(3) i t

r a b(2) b(4) i t

r a b(3) b(4) i t
```

Hence,

```
Total = 3
```

---

# Java Memoization Solution

```java
class Solution {

    public int solve(int i, int j, String s, String t, int[][] dp) {

        int m = s.length();
        int n = t.length();

        // Target completely matched
        if (j == n)
            return 1;

        // Source exhausted
        if (i == m)
            return 0;

        if (dp[i][j] != -1)
            return dp[i][j];

        if (s.charAt(i) == t.charAt(j)) {

            return dp[i][j] =
                    solve(i + 1, j + 1, s, t, dp)
                  + solve(i + 1, j, s, t, dp);
        }

        return dp[i][j] =
                solve(i + 1, j, s, t, dp);
    }

    public int numDistinct(String s, String t) {

        int[][] dp = new int[s.length()][t.length()];

        for (int i = 0; i < s.length(); i++) {
            Arrays.fill(dp[i], -1);
        }

        return solve(0, 0, s, t, dp);
    }
}
```

---

# Complexity Analysis

### Time Complexity

```
O(m × n)
```

Each state `(i, j)` is computed only once.

---

### Space Complexity

```
O(m × n)
```

For the DP table.

Recursion stack:

```
O(m + n)
```

---

# Key Takeaways

- `dp[i][j]` stores the number of ways to form `t[j...]` using `s[i...]`.
- If characters match:
  - **Take** the character.
  - **Skip** the character.
  - Add both answers.
- If characters don't match:
  - Skip the current character of `s`.
- Base cases:
  - `j == n` → `1` (target successfully formed).
  - `i == m` → `0` (source exhausted before target).
- The answer is `solve(0,0)`.
