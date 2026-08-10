# Subarray Sum Equals K

## Problem Statement

Given an integer array `nums` and an integer `k`, return the **total number of subarrays** whose sum is equal to `k`.

A subarray is a **contiguous** part of an array.

### Example

```text
Input:
nums = [1, 2, 3]
k = 3

Output:
2
```

The two subarrays are:

```text
[1, 2] → 3
[3]    → 3
```

---

## Approach

### Prefix Sum + HashMap

We maintain a running `prefixSum`.

Suppose:

```text
currentPrefixSum - previousPrefixSum = k
```

Then:

```text
previousPrefixSum = currentPrefixSum - k
```

So for every position, we check whether:

```java
prefixSum - k
```

already exists in the HashMap.

The HashMap stores:

```text
prefixSum → frequency
```

The frequency is important because the same prefix sum can occur multiple times, and each occurrence can form a valid subarray.

---

## Algorithm

1. Create a `HashMap` to store prefix sums and their frequencies.
2. Initialize:

   ```java
   map.put(0, 1);
   ```

   This handles subarrays that start from index `0`.
3. Traverse the array.
4. Add the current number to `prefixSum`.
5. Check whether `prefixSum - k` exists in the map.
6. If it exists, add its frequency to the answer.
7. Store/increment the current `prefixSum` in the map.
8. Return the count.

---

## Java Code

```java
class Solution {
    public int subarraySum(int[] nums, int k) {

        HashMap<Integer, Integer> map = new HashMap<>();

        // Prefix sum 0 occurs once
        map.put(0, 1);

        int prefixSum = 0;
        int count = 0;

        for (int num : nums) {

            prefixSum += num;

            // Check if a previous prefix sum exists
            if (map.containsKey(prefixSum - k)) {
                count += map.get(prefixSum - k);
            }

            // Store current prefix sum
            map.put(prefixSum,
                    map.getOrDefault(prefixSum, 0) + 1);
        }

        return count;
    }
}
```

---

## Dry Run

### Input

```text
nums = [1, 2, 3]
k = 3
```

Initially:

```text
prefixSum = 0
count = 0

map = {0=1}
```

### Step 1: num = 1

```text
prefixSum = 1

prefixSum - k
= 1 - 3
= -2
```

`-2` is not present.

```text
map = {0=1, 1=1}
```

---

### Step 2: num = 2

```text
prefixSum = 3

prefixSum - k
= 3 - 3
= 0
```

`0` exists in the map.

```text
count = 1
```

This represents:

```text
[1, 2]
```

Update map:

```text
map = {0=1, 1=1, 3=1}
```

---

### Step 3: num = 3

```text
prefixSum = 6

prefixSum - k
= 6 - 3
= 3
```

`3` exists.

```text
count = 2
```

This represents:

```text
[3]
```

Final answer:

```text
2
```

---

## Why `map.put(0, 1)`?

This is an important part of the solution.

Consider:

```text
nums = [3]
k = 3
```

At the first element:

```text
prefixSum = 3
```

We need:

```text
prefixSum - k
= 3 - 3
= 0
```

Since the map initially contains:

```text
0 → 1
```

we count `[3]` as a valid subarray.

---

## Why Not Sliding Window?

A normal sliding window does not work when negative numbers are allowed.

Example:

```text
nums = [1, -1, 1]
k = 1
```

Adding or removing an element does not always make the sum increase or decrease predictably.

Therefore, we use:

```text
Prefix Sum + HashMap
```

instead of a normal sliding window.

---

## Complexity

```text
Time Complexity:  O(n)
Space Complexity: O(n)
```

Each element is processed once, and the HashMap stores at most `O(n)` prefix sums.

---

## Pattern to Remember

```text
Subarray Sum = K
        ↓
Prefix Sum
        ↓
Need previousPrefix
        ↓
currentPrefix - previousPrefix = K
        ↓
previousPrefix = currentPrefix - K
        ↓
HashMap
```

### Key Formula

```text
previousPrefixSum = currentPrefixSum - k
```

This is a very important **Prefix Sum + HashMap** pattern for coding interviews and placement drives.
