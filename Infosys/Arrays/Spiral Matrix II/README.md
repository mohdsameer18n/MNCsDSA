# LeetCode 59 - Spiral Matrix II

## Problem Statement

Given a positive integer `n`, generate an `n × n` matrix filled with numbers from **1** to **n²** in **spiral order**.

---

## Examples

### Example 1

**Input**
```text
n = 3
```

**Output**
```text
[
 [1, 2, 3],
 [8, 9, 4],
 [7, 6, 5]
]
```

### Example 2

**Input**
```text
n = 1
```

**Output**
```text
[
 [1]
]
```

---

## Intuition

Instead of reading the matrix in spiral order, we **fill** the matrix in spiral order.

We maintain four boundaries:

- `srow` → Starting row
- `erow` → Ending row
- `scol` → Starting column
- `ecol` → Ending column

Starting with `num = 1`, we fill:

1. Top row (Left → Right)
2. Right column (Top → Bottom)
3. Bottom row (Right → Left)
4. Left column (Bottom → Top)

After completing one layer, we move the boundaries inward and continue until the entire matrix is filled.

---

## Approach

1. Create an `n × n` matrix.
2. Initialize:
   - `srow = 0`
   - `erow = n - 1`
   - `scol = 0`
   - `ecol = n - 1`
   - `num = 1`
3. Fill the top row.
4. Fill the right column.
5. Fill the bottom row (if more than one row exists).
6. Fill the left column (if more than one column exists).
7. Move all four boundaries inward.
8. Repeat until all cells are filled.

---

## Algorithm

```text
Initialize matrix[n][n]

srow = 0
erow = n - 1
scol = 0
ecol = n - 1
num = 1

while (srow <= erow && scol <= ecol)

    Fill top row

    Fill right column

    Fill bottom row (if srow != erow)

    Fill left column (if scol != ecol)

    srow++
    erow--
    scol++
    ecol--

Return matrix
```

---

## Java Solution

```java
class Solution {
    public int[][] generateMatrix(int n) {

        int[][] matrix = new int[n][n];

        int srow = 0, scol = 0;
        int erow = n - 1, ecol = n - 1;

        int num = 1;

        while (srow <= erow && scol <= ecol) {

            // Top Row
            for (int j = scol; j <= ecol; j++) {
                matrix[srow][j] = num++;
            }

            // Right Column
            for (int i = srow + 1; i <= erow; i++) {
                matrix[i][ecol] = num++;
            }

            // Bottom Row
            for (int j = ecol - 1; j >= scol; j--) {
                if (srow == erow) break;
                matrix[erow][j] = num++;
            }

            // Left Column
            for (int i = erow - 1; i >= srow + 1; i--) {
                if (scol == ecol) break;
                matrix[i][scol] = num++;
            }

            srow++;
            erow--;
            scol++;
            ecol--;
        }

        return matrix;
    }
}
```

---

## Dry Run

### Input

```text
n = 3
```

### Step 1

```text
1 2 3
0 0 4
0 0 5
```

### Step 2

```text
1 2 3
8 0 4
7 6 5
```

### Step 3

```text
1 2 3
8 9 4
7 6 5
```

---

## Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(n²)** |
| Space | **O(1)** (excluding the output matrix) |

---

## Key Takeaways

- Process the matrix **layer by layer**.
- Use **four boundaries** to control the spiral.
- Carefully handle **single row** and **single column** cases to avoid overwriting values.
- Every cell is filled exactly once, making the solution optimal.

---

## Tags

- Matrix
- Simulation
- Spiral Traversal
- Arrays

---

## LeetCode

**Problem Number:** 59  
**Problem Name:** Spiral Matrix II
