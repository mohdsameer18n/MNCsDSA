# Find Greatest Common Divisor of Array

**LeetCode Problem:** [Find Greatest Common Divisor of Array](https://leetcode.com/problems/find-greatest-common-divisor-of-array/)

## Problem Description

Given an integer array `nums`, find the **greatest common divisor (GCD)** of the smallest number and the largest number in the array.

### Example

```text
Input: nums = [2,5,6,9,10]

Smallest number = 2
Largest number = 10

GCD(2, 10) = 2

Output: 2
```

---

## Approach

We only need to find:

1. The **minimum** element in the array.
2. The **maximum** element in the array.
3. Calculate the GCD of these two values using **Euclid's Algorithm**.

### Euclid's Algorithm

The GCD can be calculated using:

```text
gcd(a, b) = gcd(b, a % b)
```

Continue until `b == 0`.

Then `a` is the GCD.

---

## Algorithm

```text
1. Find minimum value in nums.
2. Find maximum value in nums.
3. Calculate gcd(min, max).
4. Return the result.
```

---

## Java Solution

```java
class Solution {

    public int findGCD(int[] nums) {

        int min = nums[0];
        int max = nums[0];

        // Find minimum and maximum
        for (int num : nums) {
            min = Math.min(min, num);
            max = Math.max(max, num);
        }

        // Calculate GCD using Euclid's Algorithm
        while (max != 0) {
            int temp = max;
            max = min % max;
            min = temp;
        }

        return min;
    }
}
```

---

## Dry Run

### Input

```text
nums = [2, 5, 6, 9, 10]
```

### Step 1: Find minimum and maximum

```text
min = 2
max = 10
```

### Step 2: Calculate GCD

```text
gcd(2, 10)

10 % 2 = 0
```

So:

```text
GCD = 2
```

### Output

```text
2
```

---

## Another Example

```text
nums = [7, 5, 6, 8, 3]
```

Minimum:

```text
min = 3
```

Maximum:

```text
max = 8
```

Calculate:

```text
gcd(3, 8)

8 % 3 = 2
3 % 2 = 1
2 % 1 = 0
```

Therefore:

```text
GCD = 1
```

---

## Complexity Analysis

Let `n` be the length of the array.

### Time Complexity

Finding minimum and maximum:

```text
O(n)
```

Euclid's Algorithm:

```text
O(log(max))
```

Overall:

```text
O(n + log(max))
```

Since `O(n)` dominates:

```text
O(n)
```

### Space Complexity

Only a few variables are used:

```text
O(1)
```

---

## Key Point

The problem does **not** ask for the GCD of every element.

It specifically asks for:

```text
GCD(smallest element, largest element)
```

So first find `min` and `max`, then apply Euclid's Algorithm.
