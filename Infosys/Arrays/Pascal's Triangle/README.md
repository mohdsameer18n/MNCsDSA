# Pascal's Triangle

## Problem Statement

Given an integer `numRows`, generate the first `numRows` of Pascal's Triangle.

In Pascal's Triangle:

* The first and last element of every row is `1`.
* Every middle element is the sum of the two elements directly above it.

### Example

**Input**

```text
numRows = 5
```

**Output**

```text
[
    [1],
    [1, 1],
    [1, 2, 1],
    [1, 3, 3, 1],
    [1, 4, 6, 4, 1]
]
```

## Approach

We build the triangle row by row.

For every row:

1. Add `1` as the first element.
2. Calculate the middle elements using the previous row:

   ```java
   res.get(i - 1).get(j - 1) + res.get(i - 1).get(j)
   ```
3. Add `1` as the last element.
4. Add the completed row to `res`.

### Java Solution

```java
import java.util.*;

class Solution {
    public List<List<Integer>> generate(int numRows) {

        List<List<Integer>> res = new ArrayList<>();

        for (int i = 0; i < numRows; i++) {

            List<Integer> row = new ArrayList<>();

            // First element
            row.add(1);

            // Middle elements
            for (int j = 1; j < i; j++) {
                row.add(
                    res.get(i - 1).get(j - 1) +
                    res.get(i - 1).get(j)
                );
            }

            // Last element
            if (i > 0) {
                row.add(1);
            }

            res.add(row);
        }

        return res;
    }
}
```

## Example Dry Run

For `numRows = 5`:

```text
i = 0 → [1]

i = 1 → [1, 1]

i = 2 → [1, 2, 1]
         1 + 1 = 2

i = 3 → [1, 3, 3, 1]
         1 + 2 = 3
         2 + 1 = 3

i = 4 → [1, 4, 6, 4, 1]
         1 + 3 = 4
         3 + 3 = 6
         3 + 1 = 4
```

## Complexity

* **Time:** `O(numRows²)`
* **Space:** `O(numRows²)` because the complete triangle is stored.

## Key Formula

For every middle element:

```java
previousRow[j - 1] + previousRow[j]
```

For example:

```text
Previous row:  1  3  3  1
                   ↘ ↙
                     6
```

So:

```text
3 + 3 = 6
```
