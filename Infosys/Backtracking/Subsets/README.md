# Subsets

## Problem

Given an integer array `nums` containing unique elements, return **all possible subsets** (the power set).

The solution set must not contain duplicate subsets.

### Example

**Input:**

```text
nums = [1, 2, 3]
```

**Output:**

```text
[[], [1], [2], [3], [1, 2], [1, 3], [2, 3], [1, 2, 3]]
```

For an array of `n` elements, there are exactly:

```text
2^n
```

possible subsets.

---

## Approach

We use **Backtracking**.

For every element, we have two choices:

1. Include the element in the current subset.
2. Skip the element.

The backtracking process follows:

```text
Choose → Explore → Undo
```

---

## Java Solution

```java
import java.util.*;

class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();

        backtrack(nums, 0, new ArrayList<>(), result);

        return result;
    }

    private void backtrack(int[] nums, int index,
                           List<Integer> current,
                           List<List<Integer>> result) {

        // Add the current subset
        result.add(new ArrayList<>(current));

        // Try every remaining element
        for (int i = index; i < nums.length; i++) {

            // Choose
            current.add(nums[i]);

            // Explore
            backtrack(nums, i + 1, current, result);

            // Undo
            current.remove(current.size() - 1);
        }
    }
}
```

---

## Dry Run

For:

```text
nums = [1, 2, 3]
```

The backtracking generates:

```text
[]
[1]
[1,2]
[1,2,3]
[1,3]
[2]
[2,3]
[3]
```

The order may differ from the expected output, which is acceptable unless a specific order is required.

---

## Complexity

There are `2^n` possible subsets.

Each subset can contain up to `n` elements.

### Time Complexity

```text
O(n × 2^n)
```

### Space Complexity

```text
O(n × 2^n)
```

This includes the space required to store all subsets.

---

## Key Concept

### Backtracking Template

```java
current.add(value);              // Choose

backtrack(...);                  // Explore

current.remove(current.size()-1); // Undo
```

This **Choose → Explore → Undo** pattern is commonly used in:

* Subsets
* Subsets II
* Combinations
* Combination Sum
* Permutations
* N-Queens

---

## Related LeetCode Problems

* [Subsets - LeetCode 78]
* [Subsets II - LeetCode 90]
* [Combinations - LeetCode 77]
* [Combination Sum - LeetCode 39]
* [Permutations - LeetCode 46]

---

## Key Takeaway

For an array of `n` unique elements:

```text
Number of subsets = 2^n
```

Every element has exactly **two choices**:

```text
Include → Take the element
Exclude → Skip the element
```

Understanding this problem is an important step toward mastering **Backtracking and Recursion**.
