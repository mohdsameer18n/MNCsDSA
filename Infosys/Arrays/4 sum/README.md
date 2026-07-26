#  4Sum

**Difficulty:** Medium  
**Topics:** Array, Sorting, Two Pointers

## Problem Statement

Given an integer array `nums` of `n` integers and an integer `target`, return **all unique quadruplets** `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are distinct indices
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

The solution set must not contain duplicate quadruplets.

---

## Example 1

### Input

```text
nums = [1,0,-1,0,-2,2]
target = 0
```

### Output

```text
[
  [-2,-1,1,2],
  [-2,0,0,2],
  [-1,0,0,1]
]
```

---

## Example 2

### Input

```text
nums = [2,2,2,2,2]
target = 8
```

### Output

```text
[
  [2,2,2,2]
]
```

---

# Approach (Sorting + Two Pointers)

Instead of checking every possible quadruplet (**O(n⁴)**), we optimize using **Sorting + Two Pointers**.

### Algorithm

1. Sort the array.
2. Fix the first element (`i`).
3. Fix the second element (`j`).
4. Use two pointers:
   - `left = j + 1`
   - `right = n - 1`
5. Compute the sum of four elements.
6. If the sum equals the target:
   - Add the quadruplet.
   - Move both pointers.
   - Skip duplicate values.
7. If the sum is smaller than the target:
   - Move `left` forward.
8. If the sum is greater than the target:
   - Move `right` backward.
9. Continue until all unique quadruplets are found.

---

# Java Solution

```java
import java.util.*;

class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {

        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);

        int n = nums.length;

        for (int i = 0; i < n - 3; i++) {

            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            for (int j = i + 1; j < n - 2; j++) {

                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }

                int left = j + 1;
                int right = n - 1;

                while (left < right) {

                    long sum = (long) nums[i] + nums[j] + nums[left] + nums[right];

                    if (sum == target) {

                        ans.add(Arrays.asList(
                                nums[i],
                                nums[j],
                                nums[left],
                                nums[right]
                        ));

                        left++;
                        right--;

                        while (left < right && nums[left] == nums[left - 1]) {
                            left++;
                        }

                        while (left < right && nums[right] == nums[right + 1]) {
                            right--;
                        }

                    } else if (sum < target) {
                        left++;
                    } else {
                        right--;
                    }
                }
            }
        }

        return ans;
    }
}
```

---

# Dry Run

### Input

```text
nums = [1,0,-1,0,-2,2]
target = 0
```

### Step 1: Sort the Array

```text
[-2,-1,0,0,1,2]
```

---

### Iteration 1

```text
i = -2
j = -1

left = 0
right = 2

sum = -1

Move left

sum = 0

Quadruplet:
[-2,-1,1,2]
```

---

### Iteration 2

```text
i = -2
j = 0

left = 0
right = 2

sum = 0

Quadruplet:
[-2,0,0,2]
```

---

### Iteration 3

```text
i = -1
j = 0

left = 0
right = 1

sum = 0

Quadruplet:
[-1,0,0,1]
```

---

### Final Answer

```text
[
  [-2,-1,1,2],
  [-2,0,0,2],
  [-1,0,0,1]
]
```

---

# Why Use `long`?

```java
long sum = (long) nums[i] + nums[j] + nums[left] + nums[right];
```

The sum of four integers may exceed the range of an `int`.

Example:

```text
1000000000 +
1000000000 +
1000000000 +
1000000000
```

Result:

```text
4000000000
```

This is larger than Java's maximum `int` value (`2,147,483,647`).

Using `long` prevents integer overflow.

---

# Duplicate Handling

### Skip duplicate first element

```java
if (i > 0 && nums[i] == nums[i - 1])
    continue;
```

---

### Skip duplicate second element

```java
if (j > i + 1 && nums[j] == nums[j - 1])
    continue;
```

---

### Skip duplicate left pointer

```java
while (left < right && nums[left] == nums[left - 1]) {
    left++;
}
```

---

### Skip duplicate right pointer

```java
while (left < right && nums[right] == nums[right + 1]) {
    right--;
}
```

These checks ensure that only **unique quadruplets** are added to the answer.

---

# Complexity Analysis

| Operation | Complexity |
|-----------|------------|
| Sorting | **O(n log n)** |
| Nested Loops + Two Pointers | **O(n³)** |
| Overall Time | **O(n³)** |
| Extra Space | **O(1)** (excluding output list) |

---

# Key Points

- Sort the array before processing.
- Fix the first two elements.
- Use two pointers to find the remaining two elements.
- Skip duplicates at every level.
- Use `long` to avoid integer overflow.
- Efficient solution with **O(n³)** time complexity.

---

# Related Problems

- Two Sum
- Two Sum II - Input Array Is Sorted
- 3Sum
- 3Sum Closest
- K-Sum
- Container With Most Water
```
