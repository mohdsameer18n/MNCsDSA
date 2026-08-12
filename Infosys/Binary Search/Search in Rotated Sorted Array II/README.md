# Search in Rotated Sorted Array II

## Problem Statement

Given an integer array `nums` sorted in non-decreasing order and rotated at an unknown pivot, determine whether a given `target` exists in the array.

The array may contain duplicate values.

Return `true` if the target exists; otherwise, return `false`.

### Example

```text
Input:
nums = [2,5,6,0,0,1,2]
target = 0

Output:
true
```

```text
Input:
nums = [2,5,6,0,0,1,2]
target = 3

Output:
false
```

## Approach

Use **Binary Search**.

At each step:

1. Calculate `mid`.
2. If `nums[mid] == target`, return `true`.
3. If `nums[left] == nums[mid] == nums[right]`, the sorted half cannot be determined because of duplicates. Move both pointers:

   ```java
   left++;
   right--;
   ```
4. Otherwise, determine which half is sorted.
5. If the left half is sorted:

   * Check whether `target` lies between `nums[left]` and `nums[mid]`.
   * If yes, search the left half.
   * Otherwise, search the right half.
6. Otherwise, the right half is sorted:

   * Check whether `target` lies between `nums[mid]` and `nums[right]`.
   * If yes, search the right half.
   * Otherwise, search the left half.

## Java Solution

```java
class Solution {
    public boolean search(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {
                return true;
            }

            // Duplicates make it impossible to determine
            // which half is sorted.
            if (nums[left] == nums[mid] && nums[right] == nums[mid]) {
                left++;
                right--;
            }

            // Left half is sorted
            else if (nums[left] <= nums[mid]) {

                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }

            // Right half is sorted
            else {

                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }

        return false;
    }
}
```

## Key Idea

In a rotated sorted array, normally at least one half is sorted.

```text
Left Sorted:
nums[left] <= nums[mid]

Right Sorted:
nums[mid] <= nums[right]
```

The only special case is when:

```text
nums[left] == nums[mid] == nums[right]
```

In that situation, duplicates hide the rotation point, so we safely shrink the search space:

```java
left++;
right--;
```

## Complexity

### Average Case

```text
Time: O(log n)
Space: O(1)
```

### Worst Case

Due to duplicates, the algorithm can degrade to:

```text
Time: O(n)
Space: O(1)
```

## Important Pattern

```text
nums[mid] == target
        ↓
      Found

nums[left] == nums[mid] == nums[right]
        ↓
   left++, right--

Left half sorted
        ↓
Check target range

Right half sorted
        ↓
Check target range
```
