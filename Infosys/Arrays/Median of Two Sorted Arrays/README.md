# Median of Two Sorted Arrays

**Difficulty:** Hard  
**LeetCode:** 4

## Problem Statement

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return the **median** of the two sorted arrays.

The overall run time complexity should be **O(log(m + n))**.

---

## Example 1

### Input
```text
nums1 = [1,3]
nums2 = [2]
```

### Output
```text
2.00000
```

### Explanation
Merged array: `[1,2,3]`

Median = **2**

---

## Example 2

### Input
```text
nums1 = [1,2]
nums2 = [3,4]
```

### Output
```text
2.50000
```

### Explanation
Merged array: `[1,2,3,4]`

Median = (2 + 3) / 2 = **2.5**

---

# Approach 1: Merge Two Sorted Arrays

## Idea

- Merge both sorted arrays into a new array.
- Find the middle element(s).
- Return the median.

---

## Algorithm

1. Create a merged array.
2. Merge both arrays using two pointers.
3. If total length is odd, return the middle element.
4. Otherwise, return the average of the two middle elements.

---

## Dry Run

### Input

```text
nums1 = [1,2]
nums2 = [3,4]
```

Merged Array

```text
[1,2,3,4]
```

Total length = 4

Median

```text
(2 + 3) / 2 = 2.5
```

---

## Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(m + n)** |
| Space | **O(m + n)** |

---

# Approach 2: Binary Search (Optimal)

## Idea

Instead of merging arrays, partition them such that:

- Left half contains exactly half of the elements.
- Every element in the left half is less than or equal to every element in the right half.

Binary search is performed on the **smaller array**.

---

## Partition Visualization

```text
nums1 : 1  3  8 | 9 15
nums2 : 7 11 |18 19 21 25

Left Half
1 3 8 7 11

Right Half
9 15 18 19 21 25
```

The correct partition satisfies:

```text
max(left1, left2) <= min(right1, right2)
```

---

## Algorithm

1. Always binary search on the smaller array.
2. Calculate partition indices for both arrays.
3. Compute:
   - left1
   - right1
   - left2
   - right2
4. If partition is valid:
   - Odd length → return max(left1, left2)
   - Even length → return average of max(left) and min(right)
5. Otherwise:
   - Move left or right using binary search.

---

## Dry Run

### Input

```text
nums1 = [1,3]
nums2 = [2]
```

Swap arrays since binary search should be on the smaller array.

```text
nums1 = [2]
nums2 = [1,3]
```

### Iteration 1

```text
cut1 = 0
cut2 = 2

left1 = -∞
right1 = 2

left2 = 3
right2 = +∞
```

Since

```text
left2 > right1
```

Move right.

---

### Iteration 2

```text
cut1 = 1
cut2 = 1

left1 = 2
right1 = +∞

left2 = 1
right2 = 3
```

Now

```text
2 <= 3
1 <= +∞
```

Correct partition found.

Median

```text
max(2,1) = 2
```

---

## Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(log(min(m, n)))** |
| Space | **O(1)** |

---

# Comparison

| Approach | Time Complexity | Space Complexity |
|----------|-----------------|------------------|
| Merge Arrays | **O(m + n)** | **O(m + n)** |
| Binary Search | **O(log(min(m, n)))** | **O(1)** |

---

# Key Concepts

- Two Pointer Technique
- Binary Search
- Array Partitioning
- Median Calculation
- Edge Cases using `Integer.MIN_VALUE` and `Integer.MAX_VALUE`

---

# Edge Cases

- One array is empty.
- Arrays have different sizes.
- Arrays contain duplicate elements.
- Arrays contain negative numbers.
- Total number of elements is odd.
- Total number of elements is even.

---

# Interview Tips

- The merge approach is simple and easy to implement but does not satisfy the required logarithmic time complexity.
- The binary search approach is the expected interview solution and achieves **O(log(min(m, n)))** time with **O(1)** extra space.
- Always perform binary search on the **smaller array** to avoid index out-of-bounds errors and ensure optimal performance.
