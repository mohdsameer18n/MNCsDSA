# Minimum Palindrome Partition (Partition DP)

## Problem Statement

Given a string `S`, split it into the **minimum number of contiguous substrings** such that every substring is a **palindrome**.

A palindrome is a string that reads the same forwards and backwards.

Return the **minimum number of palindromic substrings** required.

---

## Example 1

**Input**

```text
S = "aab"
```

**Output**

```text
2
```

**Explanation**

```text
aa | b
```

Both substrings are palindromes.

---

## Example 2

**Input**

```text
S = "racecar"
```

**Output**

```text
1
```

The entire string is already a palindrome.

---

# Approach

This is a **Partition DP** problem.

At every index, try every possible ending index.

If the current substring is a palindrome, make a cut and recursively solve the remaining string.

Take the minimum among all possible partitions.

---

# DP State

Let

```text
dp[i]
```

denote the **minimum number of palindromic substrings** needed for the substring starting from index `i`.

---

# Transition

For every ending index `j`:

```text
If s[i...j] is palindrome

dp[i] = min(dp[i], 1 + dp[j+1])
```

---

# Base Case

```text
If i == n

return 0
```

No characters remain.

---

# Recurrence

```text
solve(i) = min(
              1 + solve(j+1)
           )
```

for every palindrome `s[i...j]`.

---

# Algorithm

1. Start from index `0`.
2. Try every substring beginning at the current index.
3. If the substring is a palindrome:
   - Count it as one partition.
   - Solve the remaining suffix recursively.
4. Store the result using memoization.

---

# Java Code

```java
import java.util.*;

public class Main {

    public static int solve(int i, String str, int[] dp) {

        int n = str.length();

        if (i == n)
            return 0;

        if (dp[i] != -1)
            return dp[i];

        int ans = Integer.MAX_VALUE;

        for (int j = i; j < n; j++) {

            if (isPalindrome(i, j, str)) {

                ans = Math.min(ans,
                        1 + solve(j + 1, str, dp));
            }
        }

        return dp[i] = ans;
    }

    public static boolean isPalindrome(int l, int r, String str) {

        while (l < r) {

            if (str.charAt(l) != str.charAt(r))
                return false;

            l++;
            r--;
        }

        return true;
    }

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        String str = scan.nextLine();

        int[] dp = new int[str.length()];
        Arrays.fill(dp, -1);

        System.out.println(solve(0, str, dp));
    }
}
```

---

# Dry Run

## Input

```text
aab
```

### Recursive Calls

```
solve(0)

├── "a"
│      solve(1)
│
│      └── "a"
│             solve(2)
│
│             └── "b"
│                    solve(3)=0
│
│             return 1
│
│      return 2
│
└── "aa"
       solve(2)

       └── "b"
              solve(3)=0

       return 1
```

Now,

```
Option 1

a | a | b

Partitions = 3
```

```
Option 2

aa | b

Partitions = 2
```

Minimum

```
2
```

---

# DP Table

```
dp[2] = 1
dp[1] = 2
dp[0] = 2
```

---

# Complexity Analysis

### Time Complexity

For every index:

- Try every ending index → **O(n)**
- Palindrome checking → **O(n)**

Overall:

```text
O(n³)
```

---

### Space Complexity

```text
O(n)
```

for memoization.

---

# Optimization

Precompute all palindromes using a DP table.

```
pal[i][j] = true
```

if

```
s[i...j]
```

is a palindrome.

Palindrome checking becomes **O(1)**.

New complexity:

- Precomputation → **O(n²)**
- DP → **O(n²)**

Overall:

```text
Time : O(n²)

Space : O(n²)
```

---

# Pattern Recognition

This problem belongs to **Partition DP**.

Characteristics:

- Split a string/array into valid segments.
- Every partition satisfies a condition.
- Minimize or maximize the number/cost of partitions.

General recurrence:

```text
dp[i] = min(
            cost(i,j) + dp[j+1]
         )
```

where `cost(i,j)` is valid only if the current partition satisfies the required condition.

---

# Related Problems

- LeetCode 132 – Palindrome Partitioning II
- LeetCode 131 – Palindrome Partitioning
- Matrix Chain Multiplication
- Burst Balloons
- Boolean Parenthesization
- Partition Array for Maximum Sum

---

## Tags

- Dynamic Programming
- Memoization
- Partition DP
- String
- Palindrome
