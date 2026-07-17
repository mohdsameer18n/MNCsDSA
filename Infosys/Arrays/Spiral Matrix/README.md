# Spiral Matrix

## Problem Statement

Given an `m x n` matrix, return all elements of the matrix in **spiral order**.

### Example 1

**Input**
```text
matrix = [
 [1,2,3],
 [4,5,6],
 [7,8,9]
]
```

**Output**
```text
[1,2,3,6,9,8,7,4,5]
```

### Example 2

**Input**
```text
matrix = [
 [1,2,3,4],
 [5,6,7,8],
 [9,10,11,12]
]
```

**Output**
```text
[1,2,3,4,8,12,11,10,9,5,6,7]
```

---

## Approach

Use four boundaries to traverse the matrix layer by layer.

- `srow` → Starting row
- `erow` → Ending row
- `scol` → Starting column
- `ecol` → Ending column

For each layer:

1. Traverse the **top row** from left to right.
2. Traverse the **right column** from top to bottom.
3. Traverse the **bottom row** from right to left (if more than one row remains).
4. Traverse the **left column** from bottom to top (if more than one column remains).
5. Move all four boundaries inward and repeat until all elements are visited.

---

## Algorithm

1. Initialize four boundaries:
   - `srow = 0`
   - `erow = m - 1`
   - `scol = 0`
   - `ecol = n - 1`
2. Repeat while `srow <= erow` and `scol <= ecol`.
3. Traverse:
   - Top row
   - Right column
   - Bottom row
   - Left column
4. Update the boundaries:
   - `srow++`
   - `erow--`
   - `scol++`
   - `ecol--`
5. Return the result.

---

## Complexity Analysis

- **Time Complexity:** `O(m × n)`
  - Every element is visited exactly once.

- **Space Complexity:** `O(1)` (excluding the output list).

---

## Java Solution

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;

        List<Integer> ans = new ArrayList<>();

        int srow = 0, scol = 0;
        int erow = m - 1, ecol = n - 1;

        while (srow <= erow && scol <= ecol) {

            // Top row
            for (int j = scol; j <= ecol; j++) {
                ans.add(matrix[srow][j]);
            }

            // Right column
            for (int i = srow + 1; i <= erow; i++) {
                ans.add(matrix[i][ecol]);
            }

            // Bottom row
            for (int j = ecol - 1; j >= scol; j--) {
                if (srow == erow) break;
                ans.add(matrix[erow][j]);
            }

            // Left column
            for (int i = erow - 1; i >= srow + 1; i--) {
                if (scol == ecol) break;
                ans.add(matrix[i][scol]);
            }

            srow++;
            erow--;
            scol++;
            ecol--;
        }

        return ans;
    }
}
```

---

## Key Points

- Use four boundaries to process one layer at a time.
- Handle single-row and single-column cases carefully to avoid duplicates.
- Update boundaries after completing each layer.
- This is the optimal solution expected in coding interviews.

**LeetCode Problem:** 54. Spiral Matrix
