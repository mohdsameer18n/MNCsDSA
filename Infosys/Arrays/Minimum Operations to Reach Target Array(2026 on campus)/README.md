# Minimum Operations to Reach Target Array

## Idea

In one operation, we choose a value `x`.

Then **every maximal contiguous segment whose value is `x`** is updated to the corresponding values from the `target` array.

This means:

- We perform **one operation for one distinct value**.
- If the same value appears in multiple separate segments, **all of those segments are updated together**.
- Therefore, a value should be counted **only if at least one occurrence of that value needs to change**.

So the answer is simply:

> Count the number of **distinct values in `nums`** that appear at some index where `nums[i] != target[i]`.

---

## Example 1

```text
nums   = [1,2,3]
target = [2,1,3]
```

Indices that differ:

```text
1, 2
```

Distinct values from `nums` at those indices:

```text
{1,2}
```

Answer:

```text
2
```

---

## Example 2

```text
nums   = [4,1,4]
target = [5,1,4]
```

Different indices:

```text
0
```

Distinct values:

```text
{4}
```

Answer:

```text
1
```

---

## Example 3

```text
nums   = [7,3,7]
target = [5,5,9]
```

Different indices:

```text
0,1,2
```

Distinct values:

```text
{7,3}
```

Although `7` appears twice, both segments are updated in **one operation**.

Answer:

```text
2
```

---

## Algorithm

1. Create a HashSet.
2. Traverse both arrays.
3. If `nums[i] != target[i]`, insert `nums[i]` into the HashSet.
4. Return the size of the HashSet.

---

## Dry Run

```text
nums   = [7,3,7]
target = [5,5,9]

HashSet = {}

i = 0
7 != 5
HashSet = {7}

i = 1
3 != 5
HashSet = {7,3}

i = 2
7 != 9
HashSet = {7,3}

Answer = 2
```

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(n)` (HashSet)

---

## Java Solution

```java
class Solution {
    public int minOperations(int[] nums, int[] target) {

        HashSet<Integer> set = new HashSet<>();

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != target[i]) {
                set.add(nums[i]);
            }
        }

        return set.size();
    }
}
```
