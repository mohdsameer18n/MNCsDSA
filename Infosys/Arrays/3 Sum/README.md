# 15. 3Sum

**Difficulty:** Medium  
**Topic:** Array, Sorting, Two Pointers

## Problem Statement

Given an integer array `nums`, return all the **unique triplets** `[nums[i], nums[j], nums[k]]` such that:

- `i != j`
- `i != k`
- `j != k`
- `nums[i] + nums[j] + nums[k] == 0`

The solution set must not contain duplicate triplets.

---

## Example

### Input

```text
nums = [-1,0,1,2,-1,-4]
```

### Output

```text
[[-1,-1,2],[-1,0,1]]
```

### Explanation

The unique triplets whose sum is `0` are:

- `[-1, -1, 2]`
- `[-1, 0, 1]`

---

## Approach

Instead of checking every possible triplet (**O(n³)**), we can optimize using **Sorting + Two Pointers**.

### Algorithm

1. Sort the array.
2. Iterate through each element as the first element of the triplet.
3. Skip duplicate first elements.
4. Use two pointers:
   - `left = i + 1`
   - `right = n - 1`
5. Calculate the sum:
   - If sum is `0`, store the triplet.
   - Move both pointers and skip duplicates.
   - If sum is less than `0`, move `left` forward.
   - If sum is greater than `0`, move `right` backward.
6. Continue until all unique triplets are found.

---

## Java Solution

```java
import java.util.*;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {

        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2; i++) {

            if (i > 0 && nums[i] == nums[i - 1])
                continue;

            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {

                int sum = nums[i] + nums[left] + nums[right];

                if (sum == 0) {

                    ans.add(Arrays.asList(nums[i], nums[left], nums[right]));

                    left++;
                    right--;

                    while (left < right && nums[left] == nums[left - 1])
                        left++;

                    while (left < right && nums[right] == nums[right + 1])
                        right--;

                } else if (sum < 0) {
                    left++;
                } else {
                    right--;
                }
            }
        }

        return ans;
    }
}
```

---

## Dry Run

### Input

```text
nums = [-1,0,1,2,-1,-4]
```

### Step 1: Sort the Array

```text
[-4,-1,-1,0,1,2]
```

### Iteration 1

```text
i = -4

left = -1
right = 2

sum = -3
Move left

sum = -3
Move left

sum = -2
Move left

sum = -1
Move left

No triplet found
```

---

### Iteration 2

```text
i = -1

left = -1
right = 2

sum = 0

Triplet:
[-1,-1,2]

Move both pointers

left = 0
right = 1

sum = 0

Triplet:
[-1,0,1]
```

---

### Iteration 3

```text
i = -1

Duplicate value

Skip
```

---

### Final Answer

```text
[
 [-1,-1,2],
 [-1,0,1]
]
```

---

## Duplicate Handling

### Skip duplicate first element

```java
if (i > 0 && nums[i] == nums[i - 1])
    continue;
```

---

### Skip duplicate left values

```java
while (left < right && nums[left] == nums[left - 1])
    left++;
```

---

### Skip duplicate right values

```java
while (left < right && nums[right] == nums[right + 1])
    right--;
```

These checks ensure that only **unique triplets** are added to the result.

---

## Complexity Analysis

| Operation | Complexity |
|-----------|------------|
| Sorting | O(n log n) |
| Two Pointer Search | O(n²) |
| Overall Time | **O(n²)** |
| Extra Space | **O(1)** (excluding output list) |

---

## Why Sorting?

Sorting enables the **Two Pointer** technique.

- If the current sum is **too small**, move `left` to increase the sum.
- If the current sum is **too large**, move `right` to decrease the sum.

Without sorting, this optimization is not possible, and the brute-force solution would require **O(n³)** time.

---

## Key Takeaways

- Sort the array first.
- Fix one element and search the remaining pair using two pointers.
- Skip duplicates for:
  - First element (`i`)
  - Left pointer
  - Right pointer
- Time Complexity: **O(n²)**
- Space Complexity: **O(1)** (excluding the output list)

---

## Related Problems

- Two Sum
- Two Sum II - Input Array Is Sorted
- 3Sum Closest
- 4Sum
- Container With Most Water
```
