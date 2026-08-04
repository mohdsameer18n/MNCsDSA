# Longest Palindromic Substring

## Problem

Given a string `s`, return the **longest palindromic substring**.

> A **substring** is a contiguous sequence of characters.

---

## Example

### Input

```text
s = "babad"
```

### Output

```text
"bab"
```

or

```text
"aba"
```

Both answers are correct.

---

# DP Idea (Memoization)

For every substring `s[i...j]`, determine whether it is a palindrome.

Instead of checking the same substring repeatedly, store the result in a DP table.

---

# DP State

```
dp[i][j]
```

represents

> Is the substring `s[i...j]` a palindrome?

```
true

or

false
```

---

# Base Case

## Case 1

```
i >= j
```

Examples

```
a

or

""
```

A single character (or empty string) is always a palindrome.

```java
if(i >= j)
    return true;
```

---

# Transition

## Case 1

Characters are different

```
s[i] != s[j]
```

Example

```
b a
```

Not a palindrome.

```java
return false;
```

---

## Case 2

Characters are equal

```
s[i] == s[j]
```

Example

```
b a b
^     ^
```

Now check the middle substring.

```
solve(i+1, j-1)
```

If the middle is a palindrome, the entire substring is also a palindrome.

```java
return solve(i+1, j-1);
```

---

# Recurrence

```java
if(s.charAt(i) != s.charAt(j))
    return false;

return solve(i+1, j-1);
```

---

# Dry Run

## Input

```
s = "babad"
```

Indexes

```
      0 1 2 3 4
      b a b a d
```

Initially

```
start = 0

maxLen = 1
```

---

## i = 0

### j = 0

```
solve(0,0)
```

```
i >= j
```

Return

```
true
```

Length

```
1
```

Current answer

```
b
```

---

### j = 1

```
ba
```

```
b != a
```

Return

```
false
```

---

### j = 2

```
bab
```

```
b == b
```

Now check

```
solve(1,1)
```

```
true
```

Therefore

```
bab
```

Length

```
3
```

Update

```
start = 0

maxLen = 3
```

---

### j = 3

```
baba
```

```
b != a
```

Return

```
false
```

---

### j = 4

```
babad
```

```
b != d
```

Return

```
false
```

---

## i = 1

### j = 1

```
a
```

Palindrome.

Length

```
1
```

No update.

---

### j = 2

```
ab
```

Not palindrome.

---

### j = 3

```
aba
```

```
a == a
```

Check

```
solve(2,2)
```

Returns

```
true
```

Length

```
3
```

Current maximum is already `3`.

No update.

---

### j = 4

```
abad
```

Not palindrome.

---

## i = 2

```
b
```

Palindrome.

No update.

```
ba
```

Not palindrome.

```
bad
```

Not palindrome.

---

## i = 3

```
a
```

Palindrome.

```
ad
```

Not palindrome.

---

## i = 4

```
d
```

Palindrome.

---

# Final Answer

```
start = 0

maxLen = 3
```

Return

```java
s.substring(start, start + maxLen)
```

```
"bab"
```

---

# Recursion Tree

```
solve(0,2)
      |
      |
   b == b
      |
      |
solve(1,1)
      |
      |
    true
```

Another palindrome

```
solve(1,3)
      |
      |
   a == a
      |
      |
solve(2,2)
      |
      |
    true
```

---

# Java Memoization Solution

```java
class Solution {

    public boolean solve(int i, int j, String s, Boolean[][] dp) {

        if (i >= j) {
            return true;
        }

        if (dp[i][j] != null) {
            return dp[i][j];
        }

        if (s.charAt(i) != s.charAt(j)) {
            return dp[i][j] = false;
        }

        return dp[i][j] = solve(i + 1, j - 1, s, dp);
    }

    public String longestPalindrome(String s) {

        int n = s.length();

        if (n == 0) {
            return "";
        }

        Boolean[][] dp = new Boolean[n][n];

        int start = 0;
        int maxLen = 1;

        for (int i = 0; i < n; i++) {

            for (int j = i; j < n; j++) {

                if (solve(i, j, s, dp)) {

                    if (j - i + 1 > maxLen) {

                        maxLen = j - i + 1;
                        start = i;
                    }
                }
            }
        }

        return s.substring(start, start + maxLen);
    }
}
```

---

# Complexity Analysis

### Time Complexity

```
O(n²)
```

There are `n × n` possible `(i, j)` states, and each state is computed only once.

---

### Space Complexity

```
O(n²)
```

For the memoization table.

Recursion stack:

```
O(n)
```

---

# Key Takeaways

- `dp[i][j]` stores whether `s[i...j]` is a palindrome.
- Base case:
  ```
  i >= j → true
  ```
- If the first and last characters are different:
  ```
  false
  ```
- If they are equal:
  ```
  solve(i+1, j-1)
  ```
- Check every substring `(i, j)` and keep track of the longest palindrome using:
  - `start`
  - `maxLen`
- Final answer:
  ```java
  s.substring(start, start + maxLen)
  ```
