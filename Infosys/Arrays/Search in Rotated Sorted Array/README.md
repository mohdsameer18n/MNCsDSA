# 33. Search in Rotated Sorted Array

## Problem Statement

There is an integer array `nums` sorted in ascending order with **distinct values**.

Before being passed to your function, `nums` may be **rotated** at an unknown pivot index.

For example:

```text
Original:  [0,1,2,4,5,6,7]
Rotated:   [4,5,6,7,0,1,2]
```

Given the rotated array `nums` and an integer `target`, return the **index** of `target` if it exists. Otherwise, return `-1`.

You must solve it in **O(log n)** time.

---

## Example 1

**Input**

```text
nums = [4,5,6,7,0,1,2]
target = 0
```

**Output**

```text
4
```

---

## Example 2

**Input**

```text
nums = [4,5,6,7,0,1,2]
target = 3
```

**Output**

```text
-1
```

---

## Example 3

**Input**

```text
nums = [1]
target = 0
```

**Output**

```text
-1
```

---

## Constraints

- `1 <= nums.length <= 5000`
- `-10⁴ <= nums[i] <= 10⁴`
- All elements are unique.
- `nums` is sorted and possibly rotated.
- `-10⁴ <= target <= 10⁴`

---

# Approach

We use **Binary Search**.

Even though the array is rotated, **one half of the array is always sorted**.

For every iteration:

1. Find the middle element.
2. If it is the target, return its index.
3. Determine whether the left or right half is sorted.
4. Check whether the target belongs to the sorted half.
5. Continue searching in the appropriate half.

This allows us to discard half of the search space in every iteration.

---

# Algorithm

1. Initialize `left = 0` and `right = n - 1`.
2. While `left <= right`:
   - Calculate `mid`.
   - If `nums[mid] == target`, return `mid`.
   - If the left half is sorted:
     - If the target lies in the left half, move `right`.
     - Otherwise, move `left`.
   - Otherwise, the right half is sorted:
     - If the target lies in the right half, move `left`.
     - Otherwise, move `right`.
3. If the target is not found, return `-1`.

---

# Java Solution

```java
class Solution {
    public int search(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target)
                return mid;

            // Left half is sorted
            if (nums[left] <= nums[mid]) {

                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }

            } 
            // Right half is sorted
            else {

                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }

            }
        }

        return -1;
    }
}
```

---

# Dry Run

### Input

```text
nums = [4,5,6,7,0,1,2]
target = 0
```

### Iteration 1

```text
left = 0
right = 6
mid = 3

nums[mid] = 7
```

```text
4 5 6 7 0 1 2
      ↑
```

Left half is sorted.

Target is not in the left half.

Move right.

```text
left = 4
```

---

### Iteration 2

```text
left = 4
right = 6
mid = 5

nums[mid] = 1
```

```text
0 1 2
  ↑
```

Left half is sorted.

Target lies inside.

```text
right = 4
```

---

### Iteration 3

```text
left = 4
right = 4
mid = 4
```

```text
nums[mid] = 0
```

Target found.

**Answer = 4**

---

# Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(log n)** |
| Space | **O(1)** |

---

# Key Points

- Uses Binary Search.
- One half of the rotated array is always sorted.
- Decide which half contains the target.
- Discard the other half.
- Meets the required **O(log n)** time complexity.

---

## Topics

- Array
- Binary Search
- Divide and Conquer

---

## LeetCode Link

https://leetcode.com/problems/search-in-rotated-sorted-array/
