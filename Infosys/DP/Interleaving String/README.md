# Interleaving String

## Problem

Given three strings:

- `s1`
- `s2`
- `s3`

Return **true** if `s3` is formed by interleaving `s1` and `s2`.

### Interleaving Rules

- Characters from `s1` and `s2` must appear in their original order.
- We can alternate between `s1` and `s2`.
- Every character from `s1` and `s2` must be used exactly once.

---

## Example

### Input

```text
s1 = "aabcc"
s2 = "dbbca"
s3 = "aadbbcbcac"
```

### Output

```text
true
```

---

# DP Idea

At every state we know:

- Current index in `s1`
- Current index in `s2`

The current index in `s3` is automatically

```
k = i + j
```

because we have already used

- `i` characters from `s1`
- `j` characters from `s2`

Total used

```
i + j
```

---

# DP State

```
dp[i][j]
```

represents

> Can `s3[i+j...]` be formed using `s1[i...]` and `s2[j...]`?

---

# Important Check

Before recursion

```java
if(s1.length()+s2.length()!=s3.length())
    return false;
```

If lengths don't match,

answer is immediately

```
false
```

---

# Base Case

If both strings are completely used,

```java
if(i==m && j==n)
    return true;
```

then `s3` is also completely formed.

---

# Choices

Current position in `s3`

```
k=i+j
```

---

## Choice 1

Take character from `s1`

Possible only if

```java
s1.charAt(i)==s3.charAt(k)
```

Move

```
(i+1,j)
```

---

## Choice 2

Take character from `s2`

Possible only if

```java
s2.charAt(j)==s3.charAt(k)
```

Move

```
(i,j+1)
```

---

# Recurrence

```java
boolean ans = false;

if(i < m && s1.charAt(i) == s3.charAt(i+j))
{
    ans = solve(i+1, j);
}

if(!ans && j < n && s2.charAt(j) == s3.charAt(i+j))
{
    ans = solve(i, j+1);
}

return ans;
```

---

# Dry Run

## Input

```
s1 = "aabcc"

s2 = "dbbca"

s3 = "aadbbcbcac"
```

Indexes

```
s1 : a a b c c
     0 1 2 3 4

s2 : d b b c a
     0 1 2 3 4

s3 : a a d b b c b c a c
     0 1 2 3 4 5 6 7 8 9
```

---

## Step 1

```
solve(0,0)

k=0
```

```
s3[0]='a'
```

Matches

```
s1[0]
```

Go

```
solve(1,0)
```

---

## Step 2

```
solve(1,0)

k=1
```

```
s3[1]='a'
```

Matches

```
s1[1]
```

Go

```
solve(2,0)
```

---

## Step 3

```
solve(2,0)

k=2
```

```
s3[2]='d'
```

```
s1[2]='b'

❌
```

```
s2[0]='d'

✅
```

Go

```
solve(2,1)
```

---

## Step 4

```
solve(2,1)

k=3
```

```
s3[3]='b'
```

Both match

```
s1[2]='b'

s2[1]='b'
```

Two branches exist.

Your algorithm first explores `s1`.

---

## Step 5

```
solve(3,1)

k=4
```

```
s3[4]='b'
```

Only

```
s2[1]='b'
```

matches.

Go

```
solve(3,2)
```

---

## Step 6

```
solve(3,2)

k=5
```

```
s3[5]='c'
```

Only

```
s1[3]='c'
```

matches.

Go

```
solve(4,2)
```

---

## Step 7

```
solve(4,2)

k=6
```

```
s3[6]='b'
```

Only

```
s2[2]='b'
```

matches.

Go

```
solve(4,3)
```

---

## Step 8

```
solve(4,3)

k=7
```

```
s3[7]='c'
```

Both

```
s1[4]='c'

s2[3]='c'
```

match.

First try

```
solve(5,3)
```

This path eventually fails.

So recursion backtracks.

Now try

```
solve(4,4)
```

---

## Step 9

```
solve(4,4)

k=8
```

```
s3[8]='a'
```

Only

```
s2[4]='a'
```

matches.

Go

```
solve(4,5)
```

---

## Step 10

```
solve(4,5)

k=9
```

```
s3[9]='c'
```

Matches

```
s1[4]
```

Go

```
solve(5,5)
```

---

## Base Case

```
i==m

j==n
```

Return

```
true
```

---

# Successful Path

```
s3 : a a d b b c b c a c
      ↑ ↑ ↑ ↑ ↑ ↑ ↑ ↑ ↑ ↑

s1 : a a   b   c     c
      ↑ ↑   ↑   ↑     ↑

s2 :     d   b b c a
          ↑   ↑ ↑ ↑ ↑
```

Characters are taken while preserving the order of both strings.

---

# Recursion Tree

```
solve(0,0)
      |
      |
solve(1,0)
      |
      |
solve(2,0)
      |
      |
solve(2,1)
      |
   +---------+
   |         |
 Take s1   Take s2
   |         |
 False     True
```

Memoization avoids solving the same `(i,j)` state multiple times.

---

# Java Memoization Solution

```java
class Solution {

    public boolean solve(int i, int j,
                         String s1,
                         String s2,
                         String s3,
                         Boolean[][] dp) {

        int m = s1.length();
        int n = s2.length();

        if (i == m && j == n)
            return true;

        if (dp[i][j] != null)
            return dp[i][j];

        int k = i + j;

        boolean ans = false;

        if (i < m && s1.charAt(i) == s3.charAt(k)) {
            ans = solve(i + 1, j, s1, s2, s3, dp);
        }

        if (!ans && j < n && s2.charAt(j) == s3.charAt(k)) {
            ans = solve(i, j + 1, s1, s2, s3, dp);
        }

        return dp[i][j] = ans;
    }

    public boolean isInterleave(String s1, String s2, String s3) {

        int m = s1.length();
        int n = s2.length();

        if (m + n != s3.length())
            return false;

        Boolean[][] dp = new Boolean[m + 1][n + 1];

        return solve(0, 0, s1, s2, s3, dp);
    }
}
```

---

# Complexity

### Time Complexity

```
O(m × n)
```

Each state `(i,j)` is computed only once.

---

### Space Complexity

```
O(m × n)
```

For the DP table.

Recursion stack:

```
O(m+n)
```

---

# Key Takeaways

- `dp[i][j]` tells whether the remaining part of `s3` can be formed using `s1[i...]` and `s2[j...]`.
- The current index in `s3` is always:
  ```
  k = i + j
  ```
- If `s3[k]` matches `s1[i]`, move to `(i+1, j)`.
- If `s3[k]` matches `s2[j]`, move to `(i, j+1)`.
- If both match, try both paths.
- Memoization prevents recomputing the same state.
- Always verify:
  ```
  s1.length() + s2.length() == s3.length()
  ```
  before starting recursion.
