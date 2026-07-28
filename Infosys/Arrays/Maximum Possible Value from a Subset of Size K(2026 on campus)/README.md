# Maximum Possible Value from a Subset of Size K

## Problem Statement

Given an integer array `arr` of size `N` and an integer `K`, choose any subset of size `K`.

- Multiply the **smallest element** of that subset by **-1**.
- Then subtract that value from the **largest element** of the same subset.

Return the **maximum possible value** obtainable over all subsets of size `K`.

---

## Example 1

**Input**

```text
arr = [3, 7, 1, 9, 4]
K = 3
```

**Output**

```text
13
```

**Explanation**

Choose the subset:

```text
[9, 7, 4]
```

- Largest = 9
- Smallest = 4

Expression:

```text
9 - (-4)
= 9 + 4
= 13
```

---

## Example 2

**Input**

```text
arr = [5, 5, 5, 5]
K = 2
```

**Output**

```text
10
```

**Explanation**

Largest = 5

Smallest = 5

```text
5 - (-5)
= 10
```

---

# Observation

The given expression

```text
largest - (-smallest)
```

can be simplified as

```text
largest + smallest
```

So our task is to maximize:

```text
largest + smallest
```

---

# Approach

To maximize the answer:

1. The **largest element** should always be the maximum element of the array.
2. The subset must contain exactly **K elements**.
3. To maximize the smallest element of the subset, choose the next largest `(K-1)` elements along with the maximum.

Therefore,

- Sort the array in **descending order**.
- The answer becomes

```text
arr[0] + arr[K-1]
```

because

- `arr[0]` → largest element
- `arr[K-1]` → smallest among the chosen top K elements

---

# Algorithm

1. Sort the array in descending order.
2. Return

```text
arr[0] + arr[K-1]
```

---

# Java Solution

```java
import java.util.*;

class Solution {
    public static long maxValue(int[] arr, int k) {

        Integer[] nums = Arrays.stream(arr)
                               .boxed()
                               .toArray(Integer[]::new);

        Arrays.sort(nums, Collections.reverseOrder());

        return (long) nums[0] + nums[k - 1];
    }
}
```

---

# Simpler Java Solution

```java
import java.util.*;

class Solution {
    public static long maxValue(int[] arr, int k) {

        Arrays.sort(arr);

        int n = arr.length;

        return (long) arr[n - 1] + arr[n - k];
    }
}
```

---

# Dry Run

### Input

```text
arr = [3,7,1,9,4]
k = 3
```

Sorted ascending

```text
[1,3,4,7,9]
```

Largest element

```text
9
```

Smallest among top 3 elements

```text
4
```

Answer

```text
9 + 4 = 13
```

---

# Complexity Analysis

| Operation | Complexity |
|-----------|------------|
| Sorting | O(N log N) |
| Answer Calculation | O(1) |
| Overall | O(N log N) |
| Extra Space | O(1) *(ascending sort)* |

---

# Key Insight

The expression

```text
largest - (-smallest)
```

is simply

```text
largest + smallest
```

To maximize it:

- Always include the **largest element** of the array.
- Maximize the subset's **smallest element** by selecting the **top K largest elements**.

Thus the answer is:

```text
Sorted Ascending:
arr[n-1] + arr[n-k]

OR

Sorted Descending:
arr[0] + arr[k-1]
```
