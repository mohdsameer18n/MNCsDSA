# Permutations – Backtracking

## Problem

Given an array of distinct integers `nums`, return **all possible permutations** of the array.

A permutation is an arrangement of all elements in a different order.

### Example

**Input:**

```text
nums = [1,2,3]
```

**Output:**

```text
[
 [1,2,3],
 [1,3,2],
 [2,1,3],
 [2,3,1],
 [3,1,2],
 [3,2,1]
]
```

## Approach

This problem is solved using **Backtracking**.

At every step:

1. Try each element that has not been used.
2. Add the element to the current permutation.
3. Mark the element as used.
4. Recursively build the remaining permutation.
5. Remove the element and mark it unused to explore another possibility.

A `boolean[] used` array keeps track of which elements are already included in the current permutation.

## Algorithm

```text
backtrack(nums, used, current, result)

1. If current contains all elements:
      Add a copy of current to result.
      Return.

2. For every index i:
      If nums[i] is already used:
          Continue.

      Add nums[i] to current.
      Mark nums[i] as used.

      Recursively call backtrack().

      Remove nums[i] from current.
      Mark nums[i] as unused.
```

## Java Implementation

```java
class Solution {

    public List<List<Integer>> permute(int[] nums) {

        List<Integer> current = new ArrayList<>();
        List<List<Integer>> result = new ArrayList<>();

        boolean[] used = new boolean[nums.length];

        backtrack(nums, used, current, result);

        return result;
    }

    private void backtrack(
            int[] nums,
            boolean[] used,
            List<Integer> current,
            List<List<Integer>> result) {

        if (current.size() == nums.length) {
            result.add(new ArrayList<>(current));
            return;
        }

        for (int i = 0; i < nums.length; i++) {

            if (used[i]) {
                continue;
            }

            current.add(nums[i]);
            used[i] = true;

            backtrack(nums, used, current, result);

            current.remove(current.size() - 1);
            used[i] = false;
        }
    }
}
```

## Complexity

For `n` elements, there are `n!` possible permutations.

* **Time Complexity:** `O(n × n!)`
* **Space Complexity:** `O(n)` auxiliary recursion/backtracking space
* **Output Space:** `O(n × n!)`

## Key Concept

The important idea is:

> **Choose → Explore → Undo**

For example, with `[1,2,3]`:

```text
             []
          /   |   \
        [1]  [2]  [3]
       / \    / \    / \
    [1,2] [1,3] ...
       |
    [1,2,3]
```

After exploring `[1,2,3]`, we **undo** the choice of `3`, allowing the algorithm to explore `[1,3,2]`.

## Pattern

This is a classic **Backtracking + Recursion** problem.

The same pattern can be applied to:

* Subsets
* Combination Sum
* N-Queens
* Sudoku
* Generate Parentheses
* Letter Combinations
