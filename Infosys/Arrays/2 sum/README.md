# 1. Two Sum

**Difficulty:** Easy  
**Topics:** Array, Hash Table

## Problem Statement

Given an integer array `nums` and an integer `target`, return the **indice** of the two numbers such that they add up to the target.

You may assume:

- Each input has exactly one solution.
- You may not use the same element twice.
- You can return the answer in any order.

---

## Example 1

### Input

```text
nums = [2,7,11,15]
target = 9
```

### Output

```text
[0,1]
```

### Explanation

```text
nums[0] + nums[1] = 2 + 7 = 9
```

---

## Example 2

### Input

```text
nums = [3,2,4]
target = 6
```

### Output

```text
[1,2]
```

---

## Example 3

### Input

```text
nums = [3,3]
target = 6
```

### Output

```text
[0,1]
```

---

# Approach 1: Brute Force

Check every possible pair.

### Algorithm

1. Iterate through each element.
2. Compare it with every remaining element.
3. If their sum equals the target, return their indices.

### Java Solution

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {

        int n = nums.length;

        for (int i = 0; i < n; i++) {

            for (int j = i + 1; j < n; j++) {

                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }

        return new int[]{};
    }
}
```

### Complexity

| Time | Space |
|------|-------|
| O(n²) | O(1) |

---

# Approach 2: HashMap (Optimal)

Store each number and its index in a `HashMap`.

For every element:

- Compute the complement:
  ```
  complement = target - nums[i]
  ```
- If the complement already exists in the map, return both indices.
- Otherwise, store the current number and index.

---

## Java Solution

```java
import java.util.*;

class Solution {
    public int[] twoSum(int[] nums, int target) {

        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {

            int complement = target - nums[i];

            if (map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }

            map.put(nums[i], i);
        }

        return new int[]{};
    }
}
```

---

# Dry Run

### Input

```text
nums = [2,7,11,15]
target = 9
```

### Step-by-Step

| Index | Number | Complement | HashMap | Result |
|------:|--------:|-----------:|---------|--------|
| 0 | 2 | 7 | {2=0} | Not found |
| 1 | 7 | 2 | {2=0} | Found → [0,1] |

---

### Output

```text
[0,1]
```

---

# Why HashMap?

A `HashMap` provides **O(1)** average lookup time.

Instead of searching the remaining array for the complement (**O(n)**), we directly check if it already exists in the map.

---

# Complexity Analysis

## Brute Force

| Time | Space |
|------|-------|
| O(n²) | O(1) |

## HashMap

| Time | Space |
|------|-------|
| O(n) | O(n) |

---

# Key Points

- Brute Force checks every pair.
- HashMap stores numbers with their indices.
- Find the complement using:
  ```java
  int complement = target - nums[i];
  ```
- Check the map before inserting the current element.
- Optimal Time Complexity: **O(n)**.

---

# Related Problems

- Two Sum II - Input Array Is Sorted
- 3Sum
- 4Sum
- Two Sum IV - Input is a BST
- Target Sum
```
