# Longest Palindromic Subsequence (LeetCode 516)

## Problem

Given a string `s`, return the **length of the longest palindromic subsequence**.

> A **subsequence** is formed by deleting zero or more characters **without changing the relative order** of the remaining characters.

---

## Example

### Input

```text
s = "bbbab"
```

### Output

```text
4
```

### Explanation

The longest palindromic subsequence is

```text
bbbb
```

Length = **4**

---

# DP Idea

For every substring `s[i...j]`, compute the length of the **Longest Palindromic Subsequence (LPS)**.

Store the answer so that every state is computed only once.

---

# DP State

```
dp[i][j]
```

represents

> Length of the Longest Palindromic Subsequence in substring `s[i...j]`.

---

# Base Cases

## Case 1

Single character

```java
if(i == j)
    return 1;
```

Example

```
a
```

Every single character is a palindrome.

---

## Case 2

Invalid substring

```java
if(i > j)
    return 0;
```

Example

```
""
```

No characters remain.

---

# Recurrence

## Case 1 : Characters Match

```
s.charAt(i) == s.charAt(j)
```

Example

```
b b b a b
^       ^
```

Include both characters.

```
2 + solve(i+1, j-1)
```

Java

```java
dp[i][j] = 2 + solve(i+1, j-1);
```

---

## Case 2 : Characters Don't Match

Example

```
b b b a
^     ^
```

One of them cannot belong to the same palindrome.

Two choices:

- Skip left character
- Skip right character

Take the maximum.

```java
dp[i][j] = Math.max(
                solve(i+1,j),
                solve(i,j-1)
           );
```

---

# Dry Run

## Input

```
s = "bbbab"
```

Index

```
      0 1 2 3 4
      b b b a b
```

---

## Step 1

```
solve(0,4)
```

Characters

```
b == b
```

Therefore

```
dp[0][4]

=2+solve(1,3)
```

---

## Step 2

```
solve(1,3)
```

Characters

```
b != a
```

Two choices

```
solve(2,3)

solve(1,2)
```

Take maximum.

---

## Step 3

### solve(2,3)

Characters

```
b != a
```

Again

```
max(
solve(3,3),
solve(2,2)
)
```

Both are single characters.

```
1

1
```

Therefore

```
dp[2][3]=1
```

---

## Step 4

### solve(1,2)

Characters

```
b == b
```

Therefore

```
2+solve(2,1)
```

```
solve(2,1)

i>j

return 0
```

Hence

```
dp[1][2]

=2
```

---

## Step 5

Back to

```
solve(1,3)
```

Now

```
solve(2,3)=1

solve(1,2)=2
```

Take maximum

```
dp[1][3]

=2
```

---

## Step 6

Back to

```
solve(0,4)
```

Earlier

```
b==b
```

Therefore

```
2+dp[1][3]

=2+2

=4
```

Store

```
dp[0][4]=4
```

Return

```
4
```

---

# Recursion Tree

```
solve(0,4)
        |
        |
     b == b
        |
        |
 2 + solve(1,3)
       /      \
      /        \
solve(2,3)   solve(1,2)
    /   \         |
   /     \        |
(3,3)  (2,2) 2+solve(2,1)
   |       |        |
   1       1        0
```

---

# DP Table

Computed states

| State | Value |
|-------|------:|
| dp[3][3] | 1 |
| dp[2][2] | 1 |
| dp[2][3] | 1 |
| dp[1][2] | 2 |
| dp[1][3] | 2 |
| dp[0][4] | 4 |

---

# Java Memoization Solution

```java
class Solution {

    public int solve(int i, int j, String s, Integer[][] dp) {

        if (i > j)
            return 0;

        if (i == j)
            return 1;

        if (dp[i][j] != null)
            return dp[i][j];

        if (s.charAt(i) == s.charAt(j)) {
            return dp[i][j] = 2 + solve(i + 1, j - 1, s, dp);
        }

        return dp[i][j] = Math.max(
                solve(i + 1, j, s, dp),
                solve(i, j - 1, s, dp)
        );
    }

    public int longestPalindromeSubseq(String s) {

        int n = s.length();

        Integer[][] dp = new Integer[n][n];

        return solve(0, n - 1, s, dp);
    }
}
```

---

# Complexity Analysis

### Time Complexity

```
O(n²)
```

There are `n × n` states, and each state is solved only once.

---

### Space Complexity

```
O(n²)
```

For the DP table.

Recursion stack:

```
O(n)
```

---

# Key Takeaways

- `dp[i][j]` stores the **Longest Palindromic Subsequence length** in the substring `s[i...j]`.
- If the first and last characters match, include them:
  ```
  2 + solve(i+1, j-1)
  ```
- Otherwise, skip either the left or the right character:
  ```
  max(solve(i+1, j), solve(i, j-1))
  ```
- Base cases:
  - `i > j` → `0`
  - `i == j` → `1`
- The final answer is `solve(0, n-1)`.
