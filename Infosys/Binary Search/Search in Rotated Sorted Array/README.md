# Search in Rotated Sorted Array

## Problem Statement

You are given an integer array `nums` that was originally sorted in ascending order and then rotated at an unknown position.

Given an integer `target`, return its index if it exists in the array. Otherwise, return `-1`.

All elements in `nums` are unique.

### Example

```text
Input:
nums = [4,5,6,7,0,1,2]
target = 0

Output:
4
```

---

## Approach

We use **Binary Search**.

In a rotated sorted array, at least **one half is always sorted**.

For every iteration:

1. Find `mid`.
2. If `nums[mid] == target`, return `mid`.
3. Check whether the left half is sorted.
4. If the left half is sorted:

   * Check whether the target lies inside it.
   * If yes, search left.
   * Otherwise, search right.
5. Otherwise, the right half is sorted:

   * Check whether the target lies inside it.
   * If yes, search right.
   * Otherwise, search left.

---

## Key Observation

Consider:

```text
[4,5,6,7,0,1,2]
```

There are two sorted sections:

```text
[4,5,6,7] [0,1,2]
```

At every binary-search step, one of the two halves will be sorted.

We use this property to eliminate half of the search space.

---

## Algorithm

```text
left = 0
right = n - 1

while left <= right:

    mid = left + (right - left) / 2

    if nums[mid] == target:
        return mid

    if left half is sorted:

        if target is inside left half:
            search left
        else:
            search right

    else:

        if target is inside right half:
            search right
        else:
            search left

return -1
```

---

# Java Code

```java
class Solution {
    public int search(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            // Target found
            if (nums[mid] == target) {
                return mid;
            }

            // Left half is sorted
            if (nums[left] <= nums[mid]) {

                // Target lies inside sorted left half
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }

            }

            // Right half is sorted
            else {

                // Target lies inside sorted right half
                if (nums[mid] < target && target <= nums[right]) {
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

# Full Dry Run

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

Array:

```text
[4,5,6,7,0,1,2]
 ↑        ↑     ↑
left     mid   right
```

Check:

```text
nums[mid] == target
7 == 0 → false
```

Now check whether the left half is sorted:

```text
nums[left] <= nums[mid]

4 <= 7 → true
```

So:

```text
Left half = [4,5,6,7]
```

Is target `0` inside this range?

```text
4 <= 0 && 0 < 7
```

False.

Therefore search right:

```text
left = mid + 1
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

Current part:

```text
[0,1,2]
 ↑  ↑  ↑
 L  M  R
```

Check:

```text
nums[left] <= nums[mid]

0 <= 1 → true
```

Left half is sorted:

```text
[0,1]
```

Is target `0` inside it?

```text
0 <= 0 && 0 < 1
```

True.

Therefore:

```text
right = mid - 1
right = 4
```

---

### Iteration 3

```text
left = 4
right = 4
mid = 4

nums[mid] = 0
```

Target found:

```text
return 4;
```

### Output

```text
4
```

---

# Another Example

```text
nums = [6,7,8,1,2,3,4,5]
target = 3
```

Initially:

```text
left = 0
right = 7
mid = 3

nums[mid] = 1
```

Left half:

```text
[6,7,8,1]
```

is not sorted.

Therefore the right half is sorted:

```text
[1,2,3,4,5]
```

Target `3` lies inside the right half.

So:

```text
left = mid + 1
```

We discard the entire left side.

Binary search continues on:

```text
[2,3,4,5]
```

Eventually:

```text
nums[mid] = 3
```

and we return its index.

---

# Important Conditions

## Check Left Half

```java
if (nums[left] <= nums[mid])
```

This means:

```text
LEFT HALF IS SORTED
```

Then check:

```java
if (nums[left] <= target && target < nums[mid])
```

If true:

```java
right = mid - 1;
```

Otherwise:

```java
left = mid + 1;
```

---

## Check Right Half

If the left half is not sorted, the right half is sorted.

Check:

```java
if (nums[mid] < target && target <= nums[right])
```

If true:

```java
left = mid + 1;
```

Otherwise:

```java
right = mid - 1;
```

---

# Why Binary Search Works

A normal sorted array looks like:

```text
[1,2,3,4,5,6,7]
```

A rotated sorted array might look like:

```text
[4,5,6,7,1,2,3]
```

Even though the entire array isn't sorted, at least one half around `mid` is sorted.

Therefore we can always discard one half.

```text
O(n)
   ↓
Not needed

O(log n)
   ↓
Binary Search
```

---

# Common Mistakes

### 1. Using normal binary search

This is incorrect:

```java
if (target < nums[mid]) {
    right = mid - 1;
}
```

The complete array is not sorted.

---

### 2. Forgetting the sorted-half check

Always determine:

```java
nums[left] <= nums[mid]
```

before deciding which side to search.

---

### 3. Wrong boundary conditions

For the left sorted half:

```java
nums[left] <= target && target < nums[mid]
```

For the right sorted half:

```java
nums[mid] < target && target <= nums[right]
```

---

# Complexity

```text
Time Complexity:  O(log n)
Space Complexity: O(1)
```

---

# Interview Pattern

```text
Search in Rotated Sorted Array
            ↓
      Binary Search
            ↓
   Find sorted half
            ↓
 Check whether target exists there
            ↓
      Discard half
            ↓
        Repeat
```

## Key Takeaway

> **Find which half is sorted → check whether the target belongs to that half → search the correct half.**

This is a classic **Binary Search on Rotated Sorted Array** problem and is highly important for coding interviews and placement drives.
