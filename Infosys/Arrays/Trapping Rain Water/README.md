# Trapping Rain Water

## Problem Statement

You are given an array `height` where each element represents the height of a vertical bar.

The bars have width `1`. Calculate how much rainwater can be trapped between the bars after raining.

### Example

**Input:**

```text
height = [4,2,0,3,2,5]
```

**Output:**

```text
9
```

### Explanation

The trapped water at any index depends on the tallest bar on its left and the tallest bar on its right.

```text
water[i] = min(leftMax, rightMax) - height[i]
```

For the given example:

```text
[4, 2, 0, 3, 2, 5]

Water:
   0  2  4  1  2  0

Total = 9
```

---

## Approach

We use the **Two Pointer** technique.

Maintain:

* `left` → left pointer
* `right` → right pointer
* `leftMax` → maximum height found from the left
* `rightMax` → maximum height found from the right
* `water` → total trapped water

### Algorithm

1. Initialize:

   ```text
   left = 0
   right = n - 1
   leftMax = 0
   rightMax = 0
   water = 0
   ```

2. While `left <= right`:

   * If `height[left] <= height[right]`:

     * Update `leftMax`.
     * Otherwise, add `leftMax - height[left]` to `water`.
     * Move `left`.
   * Otherwise:

     * Update `rightMax`.
     * Otherwise, add `rightMax - height[right]` to `water`.
     * Move `right`.

3. Return `water`.

---

## Java Solution

```java
class Solution {
    public int trap(int[] height) {

        int left = 0;
        int right = height.length - 1;

        int leftMax = 0;
        int rightMax = 0;

        int water = 0;

        while (left <= right) {

            if (height[left] <= height[right]) {

                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    water += leftMax - height[left];
                }

                left++;

            } else {

                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    water += rightMax - height[right];
                }

                right--;
            }
        }

        return water;
    }
}
```

---

## Dry Run

For:

```text
height = [4,2,0,3,2,5]
```

| Index | Height | Left Max | Water Added | Total |
| ----: | -----: | -------: | ----------: | ----: |
|     0 |      4 |        4 |           0 |     0 |
|     1 |      2 |        4 |           2 |     2 |
|     2 |      0 |        4 |           4 |     6 |
|     3 |      3 |        4 |           1 |     7 |
|     4 |      2 |        4 |           2 |     9 |
|     5 |      5 |        5 |           0 |     9 |

Therefore:

```text
Answer = 9
```

---

## Why Two Pointers Work

At every step, compare:

```java
height[left] <= height[right]
```

If the left height is smaller, the right side already has a boundary at least as high as the current left bar.

Therefore, the trapped water on the left depends only on `leftMax`.

Similarly, when the right height is smaller, we can calculate the trapped water using `rightMax`.

This allows us to avoid creating `leftMax[]` and `rightMax[]` arrays.

---

## Complexity

```text
Time Complexity  : O(n)
Space Complexity : O(1)
```

## Pattern

**Two Pointers + Running Maximum**

This pattern is useful when the problem requires finding information from both the left and right sides while maintaining constant extra space.

## LeetCode

[Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
