# Container With Most Water

## Problem Statement

Given an integer array `height` of length `n`, where each element represents the height of a vertical line drawn at index `i`, find two lines that together with the x-axis form a container that holds the **maximum amount of water**.

Return the maximum amount of water the container can store.

**LeetCode:** 11 - Container With Most Water

---

## Example 1

**Input**
```text
height = [1,8,6,2,5,4,8,3,7]
```

**Output**
```text
49
```

**Explanation**

Choose the lines with heights `8` and `7`.

- Width = `8 - 1 = 7`
- Height = `min(8,7) = 7`
- Area = `7 × 7 = 49`

---

## Example 2

**Input**
```text
height = [1,1]
```

**Output**
```text
1
```

---

# Approach

A brute-force solution checks every pair of lines, resulting in **O(n²)** time.

A better solution uses the **Two Pointer** technique.

### Idea

- Place one pointer at the beginning.
- Place another pointer at the end.
- Calculate the area formed by these two lines.
- Update the maximum area.
- Move the pointer having the **smaller height** inward.
- Continue until both pointers meet.

The area is calculated as:

```
Area = min(height[left], height[right]) × (right - left)
```

---

# Why Move the Smaller Height?

Suppose:

```
height[left] = 3
height[right] = 8
width = 5

Area = 3 × 5 = 15
```

If we move the taller line (`8`):

- Width decreases.
- Minimum height remains `3`.
- Area cannot increase.

Moving the shorter line gives a chance to find a taller line while trying to maximize the area.

---

# Algorithm

1. Initialize `left = 0` and `right = n - 1`.
2. Compute the current area.
3. Update the maximum area.
4. Move the pointer with the smaller height.
5. Repeat until `left >= right`.
6. Return the maximum area.

---

# Dry Run

```
height = [1,8,6,2,5,4,8,3,7]
```

| Left | Right | Width | Min Height | Area | Max Area |
|------|-------|------:|-----------:|-----:|---------:|
|0|8|8|1|8|8|
|1|8|7|7|49|49|
|1|7|6|3|18|49|
|1|6|5|8|40|49|
|2|6|4|6|24|49|
|3|6|3|2|6|49|
|4|6|2|5|10|49|
|5|6|1|4|4|49|

**Final Answer = 49**

---

# Java Solution

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int maxArea = 0;

        while (left < right) {
            int width = right - left;
            int area = Math.min(height[left], height[right]) * width;

            maxArea = Math.max(maxArea, area);

            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }
}
```

---

# Complexity Analysis

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(1)`

---

# Key Points

- Uses the **Two Pointer** technique.
- Area depends on:
  - Minimum of the two heights.
  - Distance between the two pointers.
- Always move the **shorter** line.
- Much more efficient than the brute-force `O(n²)` solution.

---

# Tags

- Array
- Two Pointers
- Greedy
- LeetCode 11
