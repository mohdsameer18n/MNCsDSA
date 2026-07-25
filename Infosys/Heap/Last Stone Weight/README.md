# 1046. Last Stone Weight

**Difficulty:** Easy  
**Topic:** Heap (Priority Queue)

---

## Problem Statement

You are given an integer array `stones`, where `stones[i]` represents the weight of the `iᵗʰ` stone.

Each turn:

1. Select the **two heaviest stones**.
2. Smash them together.
3. If both stones have the **same weight**, both are destroyed.
4. If they have **different weights**, the smaller stone is destroyed, and the larger stone becomes:

```
newStone = largerStone - smallerStone
```

Continue this process until there is **at most one stone** remaining.

Return the weight of the last remaining stone. If no stones remain, return `0`.

---

## Example 1

### Input

```text
stones = [2,7,4,1,8,1]
```

### Output

```text
1
```

### Explanation

```
Initial Stones:
[2,7,4,1,8,1]

Take 8 and 7
8 - 7 = 1
Remaining: [4,2,1,1,1]

Take 4 and 2
4 - 2 = 2
Remaining: [2,1,1,1]

Take 2 and 1
2 - 1 = 1
Remaining: [1,1,1]

Take 1 and 1
Destroyed
Remaining: [1]

Answer = 1
```

---

## Example 2

### Input

```text
stones = [1]
```

### Output

```text
1
```

---

# Approach

Since we always need the **largest two stones**, a **Max Heap (Priority Queue)** is the best data structure.

### Algorithm

1. Create a Max Heap.
2. Insert all stones into the heap.
3. While the heap contains more than one stone:
   - Remove the largest stone.
   - Remove the second largest stone.
   - If both stones are different, insert their difference back into the heap.
4. If the heap is empty, return `0`; otherwise, return the remaining stone.

---

# Dry Run

Input

```text
stones = [2,7,4,1,8,1]
```

### Step 1

```
Max Heap

[8,7,4,2,1,1]
```

### Step 2

```
Remove:
8
7

Difference = 1

Heap:
[4,2,1,1,1]
```

### Step 3

```
Remove:
4
2

Difference = 2

Heap:
[2,1,1,1]
```

### Step 4

```
Remove:
2
1

Difference = 1

Heap:
[1,1,1]
```

### Step 5

```
Remove:
1
1

Both stones destroyed.

Heap:
[1]
```

### Final Answer

```
1
```

---

# Java Solution

```java
import java.util.Collections;
import java.util.PriorityQueue;

class Solution {
    public int lastStoneWeight(int[] stones) {

        PriorityQueue<Integer> maxHeap =
                new PriorityQueue<>(Collections.reverseOrder());

        // Insert all stones
        for (int stone : stones) {
            maxHeap.offer(stone);
        }

        // Smash stones until one or none remains
        while (maxHeap.size() > 1) {

            int first = maxHeap.poll();
            int second = maxHeap.poll();

            // If weights are different, insert the remaining stone
            if (first != second) {
                maxHeap.offer(first - second);
            }
        }

        // Return the remaining stone or 0
        return maxHeap.isEmpty() ? 0 : maxHeap.poll();
    }
}
```

---

# Why does `if (first != second)` work?

```java
if (first != second) {
    maxHeap.offer(first - second);
}
```

There are two cases:

### Case 1: `first == second`

```
first = 8
second = 8
```

Both stones are destroyed.

```
Nothing is inserted back into the heap.
```

This matches the problem statement:

> If x == y, both stones are destroyed.

---

### Case 2: `first != second`

```
first = 8
second = 5
```

Remaining stone:

```
8 - 5 = 3
```

Insert `3` back into the heap.

---

# Complexity Analysis

| Operation | Complexity |
|-----------|------------|
| Build Heap | O(n log n) |
| Heap Operations | O(n log n) |
| Space | O(n) |

---

# Key Concepts

- Heap
- Priority Queue
- Max Heap
- Greedy
- Simulation

---

# Interview Tips

- Java's `PriorityQueue` is a **Min Heap** by default.
- Use `Collections.reverseOrder()` to create a **Max Heap**.
- `offer()` inserts an element into the heap.
- `poll()` removes and returns the root element.
- `peek()` returns the root without removing it.
- If two stones have equal weight, both are removed and **nothing is inserted back**.
- This problem is a classic application of a **Max Heap** for repeatedly accessing the two largest elements efficiently.
