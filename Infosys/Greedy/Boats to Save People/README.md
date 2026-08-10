# Boats to Save People

## Problem Statement

You are given an array `people` where `people[i]` represents the weight of the `i-th` person.

Each boat can carry **at most two people** and has a maximum weight limit `limit`.

Return the **minimum number of boats** required to carry everyone.

### Example

**Input**

```text
people = [3,2,2,1]
limit = 3
```

**Output**

```text
3
```

### Explanation

We can arrange the people as:

```text
[1,2] → weight = 3
[2]   → weight = 2
[3]   → weight = 3
```

Therefore, `3` boats are required.

---

## Approach

Use **Sorting + Two Pointers + Greedy**.

First, sort the array.

Maintain two pointers:

* `left` → lightest person
* `right` → heaviest person

At every step, consider the heaviest person.

### Case 1: Both people can fit

If:

```text
people[left] + people[right] <= limit
```

then put the lightest and heaviest person in the same boat.

Move both pointers:

```text
left++
right--
```

### Case 2: They cannot fit

If:

```text
people[left] + people[right] > limit
```

then the heaviest person cannot share a boat with **anyone**, because everyone else is at least as heavy as `people[left]`.

So the heaviest person goes alone:

```text
right--
```

In both cases, one boat is used.

---

## Java Solution

```java
import java.util.Arrays;

class Solution {
    public int numRescueBoats(int[] people, int limit) {

        Arrays.sort(people);

        int left = 0;
        int right = people.length - 1;
        int boats = 0;

        while (left <= right) {

            if (people[left] + people[right] <= limit) {
                left++;
            }

            right--;
            boats++;
        }

        return boats;
    }
}
```

---

## Dry Run

### Input

```text
people = [3,2,2,1]
limit = 3
```

### Step 1: Sort

```text
[1,2,2,3]
```

Pointers:

```text
left = 0  → 1
right = 3 → 3
```

Check:

```text
1 + 3 = 4 > 3
```

The heaviest person cannot share a boat.

```text
Boat 1 → [3]
```

Move:

```text
right--
```

---

### Step 2

Now:

```text
left = 0  → 1
right = 2 → 2
```

Check:

```text
1 + 2 = 3
```

They can share a boat.

```text
Boat 2 → [1,2]
```

Move:

```text
left++
right--
```

---

### Step 3

Remaining:

```text
[2]
```

One person needs one boat.

```text
Boat 3 → [2]
```

### Answer

```text
3
```

---

## Why Greedy Works

The **heaviest person is the most difficult person to accommodate**.

Suppose:

```text
people[left] + people[right] > limit
```

Since `people[left]` is already the lightest person:

```text
people[left] <= every other person
```

Therefore:

```text
every other person + people[right] > limit
```

So the heaviest person **must go alone**.

If the lightest person can fit with the heaviest person, pairing them is optimal because it saves a boat while removing the two people together.

---

## Complexity

### Time Complexity

```text
O(n log n)
```

Sorting takes `O(n log n)` and the two-pointer traversal takes `O(n)`.

### Space Complexity

```text
O(1) extra
```

Apart from the sorting implementation's internal space.

---

## Pattern

**Two Pointers + Greedy + Sorting**

### Key Pattern

```text
Sort
 ↓
Take heaviest person
 ↓
Can lightest + heaviest fit?
 ↓
YES → pair them
NO  → heaviest goes alone
 ↓
Count boat
```

This pattern is useful for problems where we need to **minimize the number of groups/pairs under a capacity constraint**.

## LeetCode

[Boats to Save People](https://leetcode.com/problems/boats-to-save-people/)
