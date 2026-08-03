# Triangle (LeetCode 120)

## Problem Statement

You are given a triangle array.

Return the **minimum path sum** from top to bottom.

From each element, you may move only to:

- Down `(i+1, j)`
- Diagonal `(i+1, j+1)`

---

## Example

### Input

```text
triangle =
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

### Output

```text
11
```

### Explanation

Minimum path

```text
      2
     /
    3
     \
      5
     /
    1
```

```
2 + 3 + 5 + 1 = 11
```

---

# Intuition

At every position `(i,j)` there are only **two choices**.

```
          (i,j)
         /     \
        /       \
(i+1,j)      (i+1,j+1)
```

Choose the path having the **minimum sum**.

---

# DP State

```
solve(i,j)
```

Represents

> Minimum path sum from `(i,j)` to the last row.

---

# Recurrence

```java
solve(i,j)=
triangle[i][j]+
Math.min(
    solve(i+1,j),
    solve(i+1,j+1)
);
```

---

# Base Case

When we reach the last row,

```java
if(i == triangle.size()-1)
    return triangle.get(i).get(j);
```

There are no more choices.

Return the current value.

---

# Memoization (Top-Down DP)

## Algorithm

1. Start from `(0,0)`.
2. Recursively compute the minimum path.
3. Store every answer in `dp`.
4. If already computed, return it.

---

## Memoization Code

```java
class Solution {

    public int solve(int i, int j,
                     List<List<Integer>> triangle,
                     Integer[][] dp) {

        if (i == triangle.size() - 1) {
            return triangle.get(i).get(j);
        }

        if (dp[i][j] != null) {
            return dp[i][j];
        }

        int down = solve(i + 1, j, triangle, dp);
        int diagonal = solve(i + 1, j + 1, triangle, dp);

        return dp[i][j] =
                triangle.get(i).get(j)
                + Math.min(down, diagonal);
    }

    public int minimumTotal(List<List<Integer>> triangle) {

        int n = triangle.size();

        Integer[][] dp = new Integer[n][n];

        return solve(0, 0, triangle, dp);
    }
}
```

---

# Tabulation (Bottom-Up DP)

## DP State

```
dp[i][j]
```

Represents

> Minimum path sum from `(i,j)` to the last row.

---

## Filling Order

Fill from

```
Bottom
   ↑
Top
```

because every cell depends on the row below.

---

## Transition

```java
dp[i][j] =
triangle[i][j] +
Math.min(
    dp[i+1][j],
    dp[i+1][j+1]
);
```

---

## Tabulation Code

```java
class Solution {

    public int minimumTotal(List<List<Integer>> triangle) {

        int n = triangle.size();

        int[][] dp = new int[n][n];

        // Copy last row
        for (int j = 0; j < n; j++) {
            dp[n-1][j] = triangle.get(n-1).get(j);
        }

        // Bottom to Top
        for (int i = n-2; i >= 0; i--) {

            for (int j = 0; j <= i; j++) {

                int down = dp[i+1][j];
                int diagonal = dp[i+1][j+1];

                dp[i][j] =
                    triangle.get(i).get(j)
                    + Math.min(down, diagonal);
            }
        }

        return dp[0][0];
    }
}
```

---

# Dry Run

### Input

```text
        2
      3   4
    6   5   7
  4   1   8   3
```

---

## Step 1

Copy last row.

```text
DP

0
0 0
0 0 0
4 1 8 3
```

---

## Step 2

Row = 2

### Cell (2,0)

```
6 + min(4,1)

=7
```

### Cell (2,1)

```
5 + min(1,8)

=6
```

### Cell (2,2)

```
7 + min(8,3)

=10
```

DP

```text
0
0 0
7 6 10
4 1 8 3
```

---

## Step 3

Row = 1

### Cell (1,0)

```
3 + min(7,6)

=9
```

### Cell (1,1)

```
4 + min(6,10)

=10
```

DP

```text
0
9 10
7 6 10
4 1 8 3
```

---

## Step 4

Row = 0

### Cell (0,0)

```
2 + min(9,10)

=11
```

Final DP

```text
11
9 10
7 6 10
4 1 8 3
```

Answer

```text
11
```

---

# Recursion Tree (Memoization)

```
solve(0,0)
│
├── solve(1,0)
│   │
│   ├── solve(2,0)
│   │   ├── solve(3,0)
│   │   └── solve(3,1)
│   │
│   └── solve(2,1)
│       ├── solve(3,1)
│       └── solve(3,2)
│
└── solve(1,1)
    │
    ├── solve(2,1)
    │
    └── solve(2,2)
```

Notice that

```
solve(2,1)
```

is called twice.

Memoization computes it only once.

---

# Complexity Analysis

| Approach | Time | Space |
|----------|------|--------|
| Memoization | **O(n²)** | **O(n²)** + recursion stack |
| Tabulation | **O(n²)** | **O(n²)** |

---

# Pattern Recognition

| Problem | Allowed Moves | Transition |
|----------|---------------|------------|
| Unique Paths | Right, Down | `down + right` |
| Minimum Path Sum | Right, Down | `grid[i][j] + min(down, right)` |
| Triangle | Down, Diagonal | `triangle[i][j] + min(down, diagonal)` |

---

# Key Takeaways

- DP State: `solve(i,j)` = minimum path sum from `(i,j)` to the last row.
- Choices: **Down** or **Diagonal**.
- Use **Math.min()** because we need the minimum path.
- Memoization stores previously computed states.
- Tabulation fills the DP table from the **bottom row to the top**.
- Final answer is:
  - **Memoization:** `solve(0,0)`
  - **Tabulation:** `dp[0][0]`
```
