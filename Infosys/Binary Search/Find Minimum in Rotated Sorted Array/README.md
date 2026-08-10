# Find Minimum in Rotated Sorted Array

## Problem Statement

You are given an array `nums` that was originally sorted in ascending order and then rotated at an unknown position.

Return the **minimum element** in the array.

### Example

```text
Input:
nums = [3,4,5,1,2]

Output:
1
```

The original sorted array was:

```text
[1,2,3,4,5]
```

After rotation:

```text
[3,4,5,1,2]
       ↑
    minimum
```

---

# Approach

We use **Binary Search**.

The important observation is:

> If `nums[mid] > nums[right]`, the minimum must be on the right side.

Otherwise:

> The minimum is at `mid` or on the left side.

We compare:

```java
nums[mid]
```

with:

```java
nums[right]
```

---

## Case 1: `nums[mid] > nums[right]`

Example:

```text
[3,4,5,1,2]
     ↑     ↑
    mid   right

5 > 2
```

The minimum must be somewhere to the **right of `mid`**.

Therefore:

```java
left = mid + 1;
```

---

## Case 2: `nums[mid] <= nums[right]`

Example:

```text
[1,2,3,4,5]
     ↑     ↑
    mid   right

3 <= 5
```

The right side is sorted, so the minimum can be:

* `mid` itself, or
* somewhere to the left.

Therefore:

```java
right = mid;
```

We use `right = mid`, not `mid - 1`, because `mid` could be the minimum.

---

# Algorithm

```text
1. Set left = 0.
2. Set right = nums.length - 1.
3. While left < right:
   a. Calculate mid.
   b. If nums[mid] > nums[right]:
        Minimum is on the right.
        left = mid + 1.
   c. Otherwise:
        Minimum is at mid or on the left.
        right = mid.
4. Return nums[left].
```

---

# Java Code

```java
class Solution {
    public int findMin(int[] nums) {

        int left = 0;
        int right = nums.length - 1;

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] > nums[right]) {

                // Minimum is on the right
                left = mid + 1;

            } else {

                // Minimum is at mid or on the left
                right = mid;
            }
        }

        return nums[left];
    }
}
```

---

# Full Dry Run

### Input

```text
nums = [3,4,5,1,2]
```

Initially:

```text
left = 0
right = 4
```

Array:

```text
[3, 4, 5, 1, 2]
 ↑        ↑     ↑
left     mid   right
```

## Iteration 1

```text
mid = 0 + (4 - 0) / 2
    = 2
```

Therefore:

```text
nums[mid] = 5
nums[right] = 2
```

Compare:

```text
5 > 2
```

So the minimum is on the right.

```java
left = mid + 1;
```

Now:

```text
left = 3
right = 4
```

---

## Iteration 2

```text
[3,4,5,1,2]
       ↑  ↑
      left right
```

Calculate:

```text
mid = 3 + (4 - 3) / 2
    = 3
```

Therefore:

```text
nums[mid] = 1
nums[right] = 2
```

Compare:

```text
1 <= 2
```

So the minimum is at `mid` or on the left.

```java
right = mid;
```

Now:

```text
left = 3
right = 3
```

---

## Stop

The condition is:

```java
while (left < right)
```

But:

```text
left = 3
right = 3
```

Therefore the loop stops.

Return:

```text
nums[left]
= nums[3]
= 1
```

### Output

```text
1
```

---

# Second Example

```text
nums = [4,5,6,7,0,1,2]
```

### Iteration 1

```text
left = 0
right = 6
mid = 3

nums[mid] = 7
nums[right] = 2
```

```text
7 > 2
```

Minimum is right:

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
nums[right] = 2
```

```text
1 <= 2
```

Minimum is at `mid` or left:

```text
right = 5
```

---

### Iteration 3

```text
left = 4
right = 5
mid = 4

nums[mid] = 0
nums[right] = 1
```

```text
0 <= 1
```

Therefore:

```text
right = 4
```

Now:

```text
left = 4
right = 4
```

Return:

```text
nums[4] = 0
```

### Output

```text
0
```

---

# Important Pattern

```text
Find Minimum
      ↓
Binary Search
      ↓
Compare nums[mid] with nums[right]
```

### Remember

```text
nums[mid] > nums[right]
        ↓
Minimum is RIGHT
        ↓
left = mid + 1
```

Otherwise:

```text
nums[mid] <= nums[right]
        ↓
Minimum is LEFT or MID
        ↓
right = mid
```

---

# Common Mistake

❌ Incorrect:

```java
right = mid - 1;
```

Why?

Because `mid` itself could be the minimum.

Example:

```text
[1,2,3,4,5]
     ↑
    mid
```

If `mid` contains the minimum, removing it would lose the answer.

Therefore use:

```java
right = mid;
```

---

# Complexity

```text
Time Complexity:  O(log n)
Space Complexity: O(1)
```

---

# Interview Takeaway

The entire problem can be remembered with one rule:

> **If `nums[mid] > nums[right]`, go right; otherwise go left including `mid`.**

```text
             nums[mid] > nums[right]?
                       /        \
                     YES         NO
                      ↓           ↓
                 left=mid+1    right=mid
```

This is a classic **Binary Search on Rotated Sorted Array** problem and is highly useful for placement interviews.
