# Pascal's Triangle II — Get Row

## Problem Statement

Given an integer `rowIndex`, return the `rowIndex`th row of Pascal's Triangle.

The row index is **0-based**.

### Example

**Input**

```text
rowIndex = 3
```

**Output**

```text
[1,3,3,1]
```

### Approach

Build Pascal's Triangle row by row until we reach `rowIndex`.

For each row:

1. Add `1` as the first element.
2. Calculate the middle elements using the previous row:

   ```java
   previousRow[j - 1] + previousRow[j]
   ```
3. Add `1` as the last element.
4. Store the row in `res`.
5. Finally, return `res.get(rowIndex)`.

### Correct Java Solution

```java
import java.util.*;

class Solution {
    public List<Integer> getRow(int rowIndex) {

        List<List<Integer>> res = new ArrayList<>();

        for (int i = 0; i <= rowIndex; i++) {

            List<Integer> row = new ArrayList<>();

            // First element
            row.add(1);

            // Middle elements
            for (int j = 1; j < i; j++) {
                row.add(
                    res.get(i - 1).get(j - 1)
                    + res.get(i - 1).get(j)
                );
            }

            // Last element
            if (i > 0) {
                row.add(1);
            }

            res.add(row);
        }

        return res.get(rowIndex);
    }
}
```

## Why Your Original Code Doesn't Work

You used:

```java
for (int i = 0; i < rowIndex; i++)
```

It should be:

```java
for (int i = 0; i <= rowIndex; i++)
```

Because if `rowIndex = 3`, you need to generate rows:

```text
0 → [1]
1 → [1,1]
2 → [1,2,1]
3 → [1,3,3,1]
```

You also used:

```java
for (int j = 1; j < rowIndex; j++)
```

It should be:

```java
for (int j = 1; j < i; j++)
```

because the number of middle elements depends on the **current row**.

Finally, this condition:

```java
if (i == rowIndex)
```

will never execute if your loop is:

```java
i < rowIndex
```

That's another reason your `ans` remains empty.

## Dry Run

For:

```text
rowIndex = 3
```

### `i = 0`

```text
row = [1]
res = [[1]]
```

### `i = 1`

```text
row = [1, 1]
res = [[1], [1,1]]
```

### `i = 2`

```text
1 + 1 = 2

row = [1, 2, 1]
```

### `i = 3`

Previous row:

```text
[1, 2, 1]
```

Calculate:

```text
1 + 2 = 3
2 + 1 = 3
```

So:

```text
row = [1, 3, 3, 1]
```

Finally:

```java
return res.get(3);
```

Output:

```text
[1, 3, 3, 1]
```

## Complexity

* **Time:** `O(rowIndex²)`
* **Space:** `O(rowIndex²)`

The `ans` list is unnecessary because we can directly return:

```java
return res.get(rowIndex);
```
