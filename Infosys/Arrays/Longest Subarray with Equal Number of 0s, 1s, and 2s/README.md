# Longest Subarray with Equal Number of 0s, 1s, and 2s

## Problem Statement

Given an array containing only **0**, **1**, and **2**, find the length of the **longest contiguous subarray** in which the number of **0s**, **1s**, and **2s** are equal.

---

## Example

### Input

```text
6
0 1 2 0 1 2
```

### Output

```text
6
```

### Explanation

The entire array contains:

* 0 → 2 times
* 1 → 2 times
* 2 → 2 times

Hence, the answer is **6**.

---

## Approach

Instead of checking every possible subarray (which would take **O(N²)**), we use a **HashMap** and **prefix differences**.

We maintain the count of:

* `zero`
* `one`
* `two`

At every index, we calculate:

```java
(zero - one, zero - two)
```

If the same pair appears again, it means the numbers of **0s**, **1s**, and **2s** have increased equally between those two indices.

Therefore, the subarray between those indices contains an equal number of **0s**, **1s**, and **2s**.

---

# Java Code

```java
import java.util.*;

class Solution {

    public int longestSubarray(int[] arr) {

        HashMap<String, Integer> map = new HashMap<>();

        int zero = 0;
        int one = 0;
        int two = 0;

        int max = 0;

        map.put("0,0", -1);

        for (int i = 0; i < arr.length; i++) {

            if (arr[i] == 0)
                zero++;
            else if (arr[i] == 1)
                one++;
            else
                two++;

            String diff = (zero - one) + "," + (zero - two);

            if (map.containsKey(diff)) {
                max = Math.max(max, i - map.get(diff));
            } else {
                map.put(diff, i);
            }
        }

        return max;
    }
}
```

---

# Algorithm

1. Initialize:

```java
zero = 0;
one = 0;
two = 0;
```

2. Create a HashMap.

```java
HashMap<String, Integer> map = new HashMap<>();
```

3. Store the initial difference.

```java
map.put("0,0", -1);
```

4. Traverse the array.

- If the element is `0`, increment `zero`.
- If the element is `1`, increment `one`.
- Otherwise, increment `two`.

5. Compute the difference.

```java
String diff = (zero - one) + "," + (zero - two);
```

6. If the difference already exists in the HashMap:

```java
max = Math.max(max, i - map.get(diff));
```

7. Otherwise, store the current index.

```java
map.put(diff, i);
```

8. Return `max`.

---

# Dry Run

Input

```text
0 1 2 0 1 2
```

Initially

| Index | zero | one | two | Difference | HashMap |
|------:|-----:|----:|----:|------------|----------|
| -1 | 0 | 0 | 0 | 0,0 | {0,0 → -1} |

---

### i = 0

Element = **0**

```text
zero = 1
one = 0
two = 0
```

Difference

```text
1,1
```

HashMap

```text
0,0 → -1
1,1 → 0
```

---

### i = 1

Element = **1**

```text
zero = 1
one = 1
two = 0
```

Difference

```text
0,1
```

HashMap

```text
0,0 → -1
1,1 → 0
0,1 → 1
```

---

### i = 2

Element = **2**

```text
zero = 1
one = 1
two = 1
```

Difference

```text
0,0
```

Already exists.

Length

```text
2 - (-1) = 3
```

Maximum

```text
max = 3
```

---

### i = 3

Element = **0**

Difference

```text
1,1
```

Already exists.

Length

```text
3 - 0 = 3
```

---

### i = 4

Element = **1**

Difference

```text
0,1
```

Already exists.

Length

```text
4 - 1 = 3
```

---

### i = 5

Element = **2**

Difference

```text
0,0
```

Already exists.

Length

```text
5 - (-1) = 6
```

Maximum

```text
max = 6
```

Output

```text
6
```

---

# How HashMap Works

The HashMap stores:

```java
Key   -> Difference (zero-one, zero-two)
Value -> First index where the difference occurred
```

Example

```text
Key      Value

0,0  -> -1
1,1  -> 0
0,1  -> 1
```

Whenever the same difference appears again, it means the counts of **0s**, **1s**, and **2s** have increased equally between those indices.

Example

```text
Index -1 : Difference = 0,0
Index  2 : Difference = 0,0
```

Subarray

```text
0 1 2
```

contains

```text
0 -> 1
1 -> 1
2 -> 1
```

Similarly,

```text
Index -1 : Difference = 0,0
Index  5 : Difference = 0,0
```

Subarray

```text
0 1 2 0 1 2
```

contains

```text
0 -> 2
1 -> 2
2 -> 2
```

---

# Why Store the First Occurrence?

Suppose the difference

```text
0,0
```

appears at

```text
Index = 2
```

and later again at

```text
Index = 10
```

Then

```text
Length = 10 - 2 = 8
```

If we overwrite the first occurrence, we lose the longest possible subarray.

Therefore, we store only the **first occurrence** of every difference.

---

# Time Complexity

- **Time:** `O(N)`
- **Space:** `O(N)`

---

# Key Concepts Used

- HashMap
- Prefix Counting
- Prefix Difference Technique
- String Keys
- Longest Subarray
- Arrays
- One-Pass Traversal
