# Two Row Maximum Sum

## Problem

Given two rows of integers, find the maximum sum of elements such that no two selected elements are adjacent within the same row.

### Input

```text
5
9 3 5 7 3
5 8 1 4 5
```

### Output

```text
29
```

## Approach

For each row, use Dynamic Programming.

At every element, there are two choices:

1. **Take** the current element and skip the previous element.
2. **Leave** the current element and consider the previous element.

The recurrence is:

```text
dp[n] = max(
    arr[n-1] + dp[n-2],
    dp[n-1]
)
```

## Java Code

```java
import java.util.*;

public class Main {

    static int solve(int[] arr, int n, int[] dp) {

        if (n <= 0) {
            return 0;
        }

        if (dp[n] != -1) {
            return dp[n];
        }

        int take = arr[n - 1] + solve(arr, n - 2, dp);
        int leave = solve(arr, n - 1, dp);

        dp[n] = Math.max(take, leave);

        return dp[n];
    }

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

        int[] row1 = new int[n];
        int[] row2 = new int[n];

        for (int i = 0; i < n; i++) {
            row1[i] = scan.nextInt();
        }

        for (int i = 0; i < n; i++) {
            row2[i] = scan.nextInt();
        }

        int[] dp1 = new int[n + 1];
        int[] dp2 = new int[n + 1];

        Arrays.fill(dp1, -1);
        Arrays.fill(dp2, -1);

        int first = solve(row1, n, dp1);
        int second = solve(row2, n, dp2);

        System.out.println(first + second);
    }
}
```

## Complexity

* **Time:** `O(n)`
* **Space:** `O(n)` for the DP arrays and recursion stack.
