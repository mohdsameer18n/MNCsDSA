# Longest Common Subsequence (LeetCode 1143)

---

# Approach 1: Recursion

## Idea

Starting from the first character of both strings:

- If the current characters match, include them in the LCS and move both pointers.
- Otherwise, try both possibilities:
  - Skip the current character of the first string.
  - Skip the current character of the second string.
- Return the maximum of the two choices.

---

## Algorithm

1. Start from `(0,0)`.
2. If either string is exhausted, return `0`.
3. If characters match:
   - Return `1 + solve(i+1, j+1)`.
4. Otherwise:
   - Skip one character from either string.
   - Return the maximum answer.

---

## Code

```java
class Solution {

    public int solve(int i, int j, String str1, String str2) {

        // Base Case
        if (i >= str1.length() || j >= str2.length()) {
            return 0;
        }

        // Characters match
        if (str1.charAt(i) == str2.charAt(j)) {
            return 1 + solve(i + 1, j + 1, str1, str2);
        }

        // Skip one character from either string
        int len1 = solve(i + 1, j, str1, str2);
        int len2 = solve(i, j + 1, str1, str2);

        return Math.max(len1, len2);
    }

    public int longestCommonSubsequence(String text1, String text2) {
        return solve(0, 0, text1, text2);
    }
}
```

### Complexity

- **Time:** `O(2^(m+n))`
- **Space:** `O(m+n)` (Recursion Stack)

---

# Approach 2: Memoization (Top-Down DP)

## Idea

The recursive solution recalculates the same `(i, j)` states multiple times.

To avoid this, store every computed state in a DP table.

Define:

```text
dp[i][j]
```

where

- `i` = current index in `text1`
- `j` = current index in `text2`

Meaning:

> `dp[i][j]` stores the length of the Longest Common Subsequence starting from indices `(i, j)`.

Before solving a state, check if it is already computed.

---

## Algorithm

1. Start from `(0,0)`.
2. If either string ends, return `0`.
3. If `dp[i][j]` already exists, return it.
4. If characters match:
   - Store `1 + solve(i+1, j+1)`.
5. Otherwise:
   - Store the maximum of:
     - Skip current character of first string.
     - Skip current character of second string.
6. Return `dp[0][0]`.

---

## Code

```java
class Solution {

    Integer[][] dp;

    public int solve(int i, int j, String str1, String str2) {

        // Base Case
        if (i >= str1.length() || j >= str2.length()) {
            return 0;
        }

        // Return stored answer
        if (dp[i][j] != null) {
            return dp[i][j];
        }

        // Characters match
        if (str1.charAt(i) == str2.charAt(j)) {
            dp[i][j] = 1 + solve(i + 1, j + 1, str1, str2);
        }
        // Characters don't match
        else {
            int len1 = solve(i + 1, j, str1, str2);
            int len2 = solve(i, j + 1, str1, str2);

            dp[i][j] = Math.max(len1, len2);
        }

        return dp[i][j];
    }

    public int longestCommonSubsequence(String text1, String text2) {

        dp = new Integer[text1.length()][text2.length()];

        return solve(0, 0, text1, text2);
    }
}
```

### Complexity

- **Time:** `O(m × n)`
- **Space:** `O(m × n)` (DP Table) + `O(m+n)` (Recursion Stack)

---

## Summary

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| Recursion | `O(2^(m+n))` | `O(m+n)` |
| Memoization | `O(m × n)` | `O(m × n)` + `O(m+n)` |
