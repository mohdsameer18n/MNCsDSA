# Missing Integer Range

## Problem Statement

You are given an integer array `nums` containing **unique integers**.

Originally, the array contained **every integer in a continuous range**, but some integers are missing. The **smallest** and **largest** integers of the original range are still present.

Return a **sorted list** of all the missing integers between the smallest and largest values.

---

## Example 1

**Input**

```text
nums = [1,4,2,5]
```

**Output**

```text
[3]
```

**Explanation**

The original range is:

```text
[1,2,3,4,5]
```

Only `3` is missing.

---

## Example 2

**Input**

```text
nums = [7,8,6,9]
```

**Output**

```text
[]
```

**Explanation**

The original range is:

```text
[6,7,8,9]
```

No numbers are missing.

---

## Example 3

**Input**

```text
nums = [5,1]
```

**Output**

```text
[2,3,4]
```

---

# Approach (HashSet)

## Idea

1. Find the **minimum** and **maximum** values in the array.
2. Store every number in a `HashSet`.
3. Iterate from `min` to `max`.
4. If a number is **not present** in the HashSet, add it to the answer.

Since HashSet provides **O(1)** average lookup, checking each number is efficient.

---

# Algorithm

1. Initialize `min` as `Integer.MAX_VALUE`.
2. Initialize `max` as `Integer.MIN_VALUE`.
3. Create a `HashSet<Integer>`.
4. Traverse the array:
   - Update `min`
   - Update `max`
   - Insert each number into the HashSet.
5. Traverse from `min` to `max`:
   - If the current number is not in the HashSet, add it to the result list.
6. Return the result.

---

# Dry Run

### Input

```text
nums = [1,4,2,5]
```

### Step 1

Find minimum and maximum.

```text
min = 1
max = 5
```

HashSet

```text
{1,2,4,5}
```

---

### Step 2

Check every number from `1` to `5`.

| Number | Present | Answer |
|---------|---------|--------|
|1|Yes|[]|
|2|Yes|[]|
|3|No|[3]|
|4|Yes|[3]|
|5|Yes|[3]|

Return

```text
[3]
```

---

# Dry Run 2

### Input

```text
nums = [5,1]
```

HashSet

```text
{1,5}
```

Range

```text
1 → 5
```

| Number | Present | Answer |
|---------|---------|--------|
|1|Yes|[]|
|2|No|[2]|
|3|No|[2,3]|
|4|No|[2,3,4]|
|5|Yes|[2,3,4]|

Return

```text
[2,3,4]
```

---

# Java Solution

```java
class Solution {
    public List<Integer> missingIntegerRange(int[] nums) {

        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;

        HashSet<Integer> set = new HashSet<>();

        for (int num : nums) {
            min = Math.min(min, num);
            max = Math.max(max, num);
            set.add(num);
        }

        List<Integer> ans = new ArrayList<>();

        for (int i = min; i <= max; i++) {
            if (!set.contains(i)) {
                ans.add(i);
            }
        }

        return ans;
    }
}
```

---

# Correctness Proof

- The HashSet contains every element present in the input array.
- Every integer in the original range lies between `min` and `max`.
- We iterate through every integer in this range exactly once.
- If a number is absent from the HashSet, it must be missing from the original range.
- Every missing number is added exactly once, and present numbers are skipped.

Therefore, the returned list contains **all and only** the missing integers in sorted order.

---

# Complexity Analysis

- **Time Complexity:** `O(n + (max - min + 1))`
  - `O(n)` to build the HashSet.
  - `O(max - min + 1)` to scan the range.

- **Space Complexity:** `O(n)`
  - HashSet stores all array elements.

---

# Key Takeaways

- Use a **HashSet** for constant-time lookups.
- Find the minimum and maximum values first.
- Scan the complete range once.
- Simple, clean, and optimal for the given constraints.
