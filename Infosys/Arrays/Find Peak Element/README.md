# 162. Find Peak Element

## Problem Statement

A peak element is an element that is **strictly greater** than its neighbors.

Given a **0-indexed** integer array `nums`, find a peak element and return its index.

If the array contains multiple peaks, return the index of **any** peak.

You may imagine that `nums[-1] = nums[n] = -∞`.

**Example 1**

```text
Input: nums = [1,2,3,1]
Output: 2
```

**Example 2**

```text
Input: nums = [1,2,1,3,5,6,4]
Output: 5
```

---

# Approach 1: Linear Scan (Brute Force)

## Intuition

A peak element is greater than both of its neighbors.

* If the array has only one element, it is the peak.
* Check if the first element is a peak.
* Check if the last element is a peak.
* Otherwise, iterate through the middle elements and return the first element that is greater than both its left and right neighbors.

---

## Algorithm

1. If the array size is `1`, return `0`.
2. If `nums[0] > nums[1]`, return `0`.
3. If `nums[n-1] > nums[n-2]`, return `n-1`.
4. Traverse from index `1` to `n-2`.
5. Return the index where:

```text
nums[i] > nums[i-1]
AND
nums[i] > nums[i+1]
```

---

## Code (Java)

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int n = nums.length;

        if (n == 1)
            return 0;

        if (nums[0] > nums[1])
            return 0;

        if (nums[n - 1] > nums[n - 2])
            return n - 1;

        for (int i = 1; i < n - 1; i++) {
            if (nums[i] > nums[i - 1] && nums[i] > nums[i + 1]) {
                return i;
            }
        }

        return -1;
    }
}
```

---

### Complexity Analysis

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(1)`

---

# Approach 2: Binary Search (Optimal)

## Intuition

Instead of checking every element, observe the slope around the middle element.

* If `nums[mid] > nums[mid + 1]`, then we are on a **descending slope**.

  * A peak must exist on the **left side**, including `mid`.
* If `nums[mid] < nums[mid + 1]`, then we are on an **ascending slope**.

  * A peak must exist on the **right side**.

By eliminating half of the search space each time, we achieve `O(log n)` time complexity.

---

## Algorithm

1. Initialize:

   * `low = 0`
   * `high = n - 1`
2. While `low < high`:

   * Compute `mid`.
   * If `nums[mid] > nums[mid + 1]`, move left:

     * `high = mid`
   * Else move right:

     * `low = mid + 1`
3. When `low == high`, return `low`.

---

## Code (Java)

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int low = 0;
        int high = nums.length - 1;

        while (low < high) {
            int mid = low + (high - low) / 2;

            if (nums[mid] > nums[mid + 1]) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }

        return low;
    }
}
```

---

## Dry Run

### Example

```text
nums = [1,2,3,1]
```

| low | high | mid | nums[mid] | nums[mid+1] | Action   |
| --- | ---- | --- | --------- | ----------- | -------- |
| 0   | 3    | 1   | 2         | 3           | low = 2  |
| 2   | 3    | 2   | 3         | 1           | high = 2 |

Now:

```text
low = high = 2
```

Return:

```text
2
```

---

### Complexity Analysis

* **Time Complexity:** `O(log n)`
* **Space Complexity:** `O(1)`

---

# Comparison

| Approach      | Time Complexity | Space Complexity |
| ------------- | --------------- | ---------------- |
| Linear Scan   | `O(n)`          | `O(1)`           |
| Binary Search | `O(log n)`      | `O(1)`           |

---

# Key Takeaways

* The linear scan is simple and easy to understand.
* The binary search solution is the expected optimal approach in interviews and on LeetCode.
* Use `while (low < high)` in the binary search solution to avoid out-of-bounds access and ensure the loop terminates correctly.
