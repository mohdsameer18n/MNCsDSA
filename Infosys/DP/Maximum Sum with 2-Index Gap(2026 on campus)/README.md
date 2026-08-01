# Maximum Sum with 2-Index Gap

## Problem Statement

Given an array of integers, find the maximum possible sum by selecting elements such that there are **at least 2 indices between any two selected elements**.

If you choose the element at index `i`, the next chosen element can only be from index `i + 3` or later.

---

## Example

### Input

```text
6
4 2 7 5 1 8
```

### Output

```text
15
```

### Explanation

Possible selections:

- `4 + 5 = 9`
- `4 + 8 = 12`
- `2 + 8 = 10`
- `7 + 8 = 15` ✅

Maximum sum = **15**

---

# Approach 1: Memoization (Top-Down DP)

## Idea

At every index, we have two choices:

1. **Include** the current element and jump to `i + 3`.
2. **Exclude** the current element and move to `i + 1`.

Store the answer of every index in a DP array so it is calculated only once.

---

## DP State

```text
solve(i)
```

Represents the maximum sum starting from index `i`.

---

## Recurrence

```text
solve(i) = max(
    arr[i] + solve(i + 3),
    solve(i + 1)
)
```

---

## Base Case

```text
if(i >= n)
    return 0;
```

---

## Flow Diagram

```text
                           solve(i)
                          /        \
                    Include        Exclude
                       |              |
             arr[i] + solve(i+3)   solve(i+1)
                       \            /
                        \          /
                     max(include, exclude)
```

---

## Memoization Code (Java)

```java
import java.util.*;

public class Main {

    public static int solve(int i, int[] arr, int[] dp) {

        if (i >= arr.length)
            return 0;

        if (dp[i] != -1)
            return dp[i];

        int include = arr[i] + solve(i + 3, arr, dp);
        int exclude = solve(i + 1, arr, dp);

        dp[i] = Math.max(include, exclude);

        return dp[i];
    }

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

        int[] arr = new int[n];

        for (int i = 0; i < n; i++)
            arr[i] = scan.nextInt();

        int[] dp = new int[n];
        Arrays.fill(dp, -1);

        System.out.println(solve(0, arr, dp));
    }
}
```

---

## Dry Run

Array

```text
Index : 0 1 2 3 4 5
Value : 4 2 7 5 1 8
```

Recursive computation

```text
solve(0)
│
├── Include = 4 + solve(3)
│
└── Exclude = solve(1)
```

Computed DP values

| Index | 0 | 1 | 2 | 3 | 4 | 5 |
|------:|--:|--:|--:|--:|--:|--:|
| dp |15|15|15|8|8|8|

Answer

```text
15
```

---

## Time Complexity

```text
O(n)
```

## Space Complexity

```text
O(n)
```

---

# Approach 2: Tabulation (Bottom-Up DP)

## Idea

Instead of recursion, build the answer from the last index.

Define

```text
dp[i]
```

where

```text
dp[i] = Maximum sum starting from index i
```

---

## Transition

```text
dp[i] = max(
    arr[i] + dp[i+3],
    dp[i+1]
)
```

---

## Base Values

```text
dp[n] = 0
dp[n+1] = 0
dp[n+2] = 0
```

Therefore,

```text
dp = new int[n+3];
```

---

## Filling Order

```text
for(i = n-1; i >= 0; i--)
```

Fill the DP array from **right to left**.

---

## Flow Diagram

```text
dp[5]
 ↑
dp[4]
 ↑
dp[3]
 ↑
dp[2]
 ↑
dp[1]
 ↑
dp[0]
```

---

## Tabulation Code (Java)

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

        int[] arr = new int[n];

        for (int i = 0; i < n; i++)
            arr[i] = scan.nextInt();

        int[] dp = new int[n + 3];

        for (int i = n - 1; i >= 0; i--) {

            int include = arr[i] + dp[i + 3];
            int exclude = dp[i + 1];

            dp[i] = Math.max(include, exclude);
        }

        System.out.println(dp[0]);
    }
}
```

---

## Dry Run

Initial DP

```text
Index : 0 1 2 3 4 5 6 7 8
dp    : 0 0 0 0 0 0 0 0 0
```

### i = 5

```text
include = 8 + dp[8] = 8
exclude = dp[6] = 0

dp[5] = 8
```

```text
dp = [0 0 0 0 0 8 0 0 0]
```

---

### i = 4

```text
include = 1 + dp[7] = 1
exclude = dp[5] = 8

dp[4] = 8
```

```text
dp = [0 0 0 0 8 8 0 0 0]
```

---

### i = 3

```text
include = 5 + dp[6] = 5
exclude = dp[4] = 8

dp[3] = 8
```

```text
dp = [0 0 0 8 8 8 0 0 0]
```

---

### i = 2

```text
include = 7 + dp[5] = 15
exclude = dp[3] = 8

dp[2] = 15
```

```text
dp = [0 0 15 8 8 8 0 0 0]
```

---

### i = 1

```text
include = 2 + dp[4] = 10
exclude = dp[2] = 15

dp[1] = 15
```

```text
dp = [0 15 15 8 8 8 0 0 0]
```

---

### i = 0

```text
include = 4 + dp[3] = 12
exclude = dp[1] = 15

dp[0] = 15
```

Final DP

```text
Index : 0  1  2  3  4  5
dp    :15 15 15  8  8  8
```

Answer

```text
15
```

---

## Time Complexity

```text
O(n)
```

## Space Complexity

```text
O(n)
```

---

# Memoization vs Tabulation

| Memoization (Top-Down) | Tabulation (Bottom-Up) |
|-------------------------|------------------------|
| Uses recursion | Uses loops |
| Starts from `solve(0)` | Starts from the last index |
| Solves only required states | Computes all states |
| DP initialized with `-1` | DP initialized with `0` |
| Uses recursion stack | No recursion stack |

---

# Summary

### DP State

```text
dp[i] = Maximum sum starting from index i
```

### Include

```text
arr[i] + dp[i+3]
```

### Exclude

```text
dp[i+1]
```

### Transition

```text
dp[i] = max(arr[i] + dp[i+3], dp[i+1])
```

This is a variation of the **House Robber** problem where, after choosing an element, you must skip the next **2 indices** (jump to `i + 3`).
