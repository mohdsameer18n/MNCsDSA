# Missing Number

## Problem Statement

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the **only number** in the range that is missing from the array.

---

## Example 1

**Input**

```text
nums = [3,0,1]
```

**Output**

```text
2
```

**Explanation**

The numbers should be:

```text
0 1 2 3
```

Missing number is `2`.

---

## Example 2

**Input**

```text
nums = [0,1]
```

**Output**

```text
2
```

---

## Example 3

**Input**

```text
nums = [9,6,4,2,3,5,7,0,1]
```

**Output**

```text
8
```

---

# Approach 1: Sum Formula (Optimal)

## Idea

The sum of numbers from `0` to `n` is

```text
n × (n + 1) / 2
```

Find the expected sum and subtract the actual array sum.

---

## Algorithm

1. Compute `expectedSum = n × (n + 1) / 2`.
2. Compute the sum of all elements.
3. Return

```text
expectedSum - actualSum
```

---

## Dry Run

### Input

```text
nums = [3,0,1]
```

```
n = 3

Expected Sum = 3 × 4 / 2 = 6

Actual Sum = 3 + 0 + 1 = 4

Missing = 6 - 4 = 2
```

Return

```text
2
```

---

## Java Solution

```java
class Solution {
    public int missingNumber(int[] nums) {

        int n = nums.length;

        int expectedSum = n * (n + 1) / 2;

        int actualSum = 0;

        for (int num : nums) {
            actualSum += num;
        }

        return expectedSum - actualSum;
    }
}
```

---

# Approach 2: XOR (Most Optimal)

## Idea

XOR has the property:

```text
a ^ a = 0
a ^ 0 = a
```

XOR all indices and all array elements.

Duplicate values cancel each other.

Only the missing number remains.

---

## Algorithm

1. Initialize `xor = n`.
2. Traverse the array:
   - `xor ^= i`
   - `xor ^= nums[i]`
3. Return `xor`.

---

## Dry Run

### Input

```text
nums = [3,0,1]
```

```
xor = 3

i = 0
xor = 3 ^ 0 ^ 3 = 0

i = 1
xor = 0 ^ 1 ^ 0 = 1

i = 2
xor = 1 ^ 2 ^ 1 = 2
```

Return

```text
2
```

---

## Java Solution

```java
class Solution {
    public int missingNumber(int[] nums) {

        int xor = nums.length;

        for (int i = 0; i < nums.length; i++) {
            xor ^= i;
            xor ^= nums[i];
        }

        return xor;
    }
}
```

---

# Correctness Proof

### Sum Formula

- The expected sum contains every number from `0` to `n`.
- The actual sum is missing exactly one number.
- Their difference is the missing number.

### XOR

- Every number appearing twice cancels out (`a ^ a = 0`).
- Only the missing number does not cancel.
- Hence the remaining XOR value is the answer.

---

# Complexity Analysis

## Sum Formula

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(1)`

## XOR

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(1)`

---

# Key Takeaways

- **Sum Formula:** Simple and intuitive.
- **XOR:** Avoids overflow and uses constant space.
- Both are optimal with **O(n)** time and **O(1)** space.
