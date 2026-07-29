# Jump Game

## Problem Statement

Given an integer array `nums`, where each element represents the **maximum jump length** from that position, determine whether you can reach the last index starting from the first index.

Return:

- `true` if the last index is reachable.
- `false` otherwise.

---

## Example

### Example 1

**Input**
```text
nums = [2,3,1,1,4]
```

**Output**
```text
true
```

**Explanation**

- Start at index `0` (jump up to `2` steps).
- Jump to index `1`.
- From index `1`, jump directly to the last index.
- Therefore, the last index is reachable.

---

### Example 2

**Input**
```text
nums = [3,2,1,0,4]
```

**Output**
```text
false
```

**Explanation**

You get stuck at index `3` because its jump length is `0`, making it impossible to reach the last index.

---

## Approach (Greedy)

Maintain the **farthest index** that can be reached while traversing the array.

1. Initialize `maxReach = 0`.
2. Traverse the array from left to right.
3. If the current index is greater than `maxReach`, that index is unreachable, so return `false`.
4. Update the farthest reachable index:
   ```java
   maxReach = Math.max(maxReach, i + nums[i]);
   ```
5. If the loop completes, return `true`.

---

## Algorithm

1. Initialize `maxReach = 0`.
2. Iterate through the array.
3. Check if the current index is reachable.
4. Update the farthest reachable position.
5. Return `true` after visiting all reachable indices.

---

## Java Solution

```java
class Solution {
    public boolean canJump(int[] nums) {
        int maxReach = 0;

        for (int i = 0; i < nums.length; i++) {

            if (i > maxReach)
                return false;

            maxReach = Math.max(maxReach, i + nums[i]);
        }

        return true;
    }
}
```

---

## Dry Run

### Input

```text
nums = [2,3,1,1,4]
```

| Index | nums[i] | maxReach Before | maxReach After |
|------:|--------:|----------------:|---------------:|
| 0 | 2 | 0 | 2 |
| 1 | 3 | 2 | 4 |
| 2 | 1 | 4 | 4 |
| 3 | 1 | 4 | 4 |
| 4 | 4 | 4 | 8 |

Since the last index is reachable, the answer is **true**.

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(1)`

---

## Key Takeaways

- This problem is solved optimally using a **Greedy** approach.
- `maxReach` stores the farthest index reachable at any point.
- If the current index becomes greater than `maxReach`, reaching the end is impossible.
- No Dynamic Programming is required because keeping track of the farthest reachable position is sufficient.

---

## Tags

- Array
- Greedy
- LeetCode 55
- Interview Preparation
