# Binary Subarrays With Sum

## Problem Statement

Given a binary array `nums` and an integer `goal`, return the number of **non-empty subarrays** whose sum is equal to `goal`.

A binary array contains only:

```text
0 and 1
```

### Example

```text
Input:
nums = [1,0,1,0,1]
goal = 2

Output:
4
```

The valid subarrays are:

```text
[1,0,1]
[1,0,1,0]
[0,1,0,1]
[1,0,1]
```

---

# Approach

## Prefix Sum + HashMap

This problem follows the same pattern as:

**Subarray Sum Equals K**

We maintain a running `prefixSum`.

Suppose:

```text
currentPrefixSum - previousPrefixSum = goal
```

Then:

```text
previousPrefixSum = currentPrefixSum - goal
```

Therefore, for every element we check:

```java
prefixSum - goal
```

in the HashMap.

The HashMap stores:

```text
prefixSum → frequency
```

The frequency is important because the same prefix sum can occur multiple times.

---

## Algorithm

1. Create a `HashMap` to store prefix sums and their frequencies.
2. Initialize:

   ```java
   map.put(0, 1);
   ```
3. Traverse the array.
4. Add the current number to `prefixSum`.
5. Calculate:

   ```text
   prefixSum - goal
   ```
6. If it exists in the map, add its frequency to `count`.
7. Store the current prefix sum in the map.
8. Return `count`.

---

# Java Code

```java
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {

        HashMap<Integer, Integer> map = new HashMap<>();

        // Prefix sum 0 occurs once
        map.put(0, 1);

        int prefixSum = 0;
        int count = 0;

        for (int num : nums) {

            prefixSum += num;

            // Check for previous prefix sum
            if (map.containsKey(prefixSum - goal)) {
                count += map.get(prefixSum - goal);
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

# Dry Run

### Input

```text
nums = [1,0,1,0,1]
goal = 2
```

Initially:

```text
prefixSum = 0
count = 0

map = {0=1}
```

### Step 1: `num = 1`

```text
prefixSum = 1

prefixSum - goal
= 1 - 2
= -1
```

`-1` is not present.

```text
map = {0=1, 1=1}
```

---

### Step 2: `num = 0`

```text
prefixSum = 1

1 - 2 = -1
```

Not found.

The prefix sum `1` occurs again:

```text
map = {0=1, 1=2}
```

---

### Step 3: `num = 1`

```text
prefixSum = 2

2 - 2 = 0
```

`0` exists once.

```text
count = 1
```

Valid subarray:

```text
[1,0,1]
```

---

### Step 4: `num = 0`

```text
prefixSum = 2

2 - 2 = 0
```

Again `0` exists once.

```text
count = 2
```

---

### Step 5: `num = 1`

```text
prefixSum = 3

3 - 2 = 1
```

Prefix sum `1` occurred **twice**.

Therefore:

```text
count += 2
```

```text
count = 4
```

Final answer:

```text
4
```

---

# Why `map.put(0, 1)`?

Consider:

```text
nums = [1]
goal = 1
```

At the first element:

```text
prefixSum = 1
```

We need:

```text
prefixSum - goal
= 1 - 1
= 0
```

The initial:

```java
map.put(0, 1);
```

allows us to count the subarray starting from index `0`.

---

# Alternative Approach

Because `nums` contains only `0` and `1`, we can also use:

```text
Exactly(goal)
=
AtMost(goal) - AtMost(goal - 1)
```

### Java Code

```java
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        return atMost(nums, goal) - atMost(nums, goal - 1);
    }

    private int atMost(int[] nums, int goal) {

        if (goal < 0) {
            return 0;
        }

        int left = 0;
        int sum = 0;
        int count = 0;

        for (int right = 0; right < nums.length; right++) {

            sum += nums[right];

            while (sum > goal) {
                sum -= nums[left];
                left++;
            }

            // Number of valid subarrays ending at right
            count += right - left + 1;
        }

        return count;
    }
}
```

---

# Why Sliding Window Works Here

Normally, sliding window does not work for arbitrary arrays containing negative numbers.

But here:

```text
nums[i] = 0 or 1
```

There are no negative numbers.

Therefore, when we expand the window, the sum never decreases unexpectedly.

This allows us to maintain:

```text
sum <= goal
```

using a sliding window.

---

# Prefix Sum vs Sliding Window

| Approach                |   Time |  Space | General                       |
| ----------------------- | -----: | -----: | ----------------------------- |
| Prefix Sum + HashMap    | `O(n)` | `O(n)` | Works with negative numbers   |
| Sliding Window / AtMost | `O(n)` | `O(1)` | Works because array is binary |

For interviews, the **Prefix Sum + HashMap approach** is the easiest to recognize and remember.

---

# Pattern to Remember

```text
Binary Subarrays With Sum
          ↓
Need count of subarrays
          ↓
Prefix Sum + HashMap
```

Or, because the array contains only `0` and `1`:

```text
Exactly(goal)
      ↓
AtMost(goal) - AtMost(goal - 1)
      ↓
Sliding Window
```

## Complexity

### Prefix Sum + HashMap

```text
Time:  O(n)
Space: O(n)
```

### Sliding Window

```text
Time:  O(n)
Space: O(1)
```

---

## Key Interview Takeaway

Remember these three related problems:

```text
Subarray Sum Equals K
        ↓
Prefix Sum + HashMap

Binary Subarrays With Sum
        ↓
Prefix Sum + HashMap
        OR
AtMost() + Sliding Window

Maximum Subarray Sum With Distinct Elements
        ↓
Sliding Window + HashSet
```

These are excellent problems for learning the difference between **Prefix Sum**, **HashMap**, and **Sliding Window** patterns.
