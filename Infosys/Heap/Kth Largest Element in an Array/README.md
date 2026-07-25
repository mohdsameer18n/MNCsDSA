# 215. Kth Largest Element in an Array

**Difficulty:** Medium  
**Topics:** Heap (Priority Queue), Sorting

---

## Problem Statement

Given an integer array `nums` and an integer `k`, return the **kᵗʰ largest** element in the array.

> **Note:** It is the **kᵗʰ largest element in sorted order**, not the kᵗʰ distinct element.

---

## Example 1

### Input

```text
nums = [3,2,1,5,6,4]
k = 2
```

### Output

```text
5
```

### Explanation

Sorted in descending order:

```text
6 5 4 3 2 1
```

The **2nd largest** element is **5**.

---

## Example 2

### Input

```text
nums = [3,2,3,1,2,4,5,5,6]
k = 4
```

### Output

```text
4
```

Sorted:

```text
6 5 5 4 3 3 2 2 1
```

The **4th largest** element is **4**.

---

# Approach 1: Max Heap

## Idea

- Create a **Max Heap**.
- Insert all elements into the heap.
- Remove (`poll`) the largest element **k−1** times.
- The next `poll()` gives the **kᵗʰ largest** element.

---

## Algorithm

1. Create a Max Heap.
2. Insert all numbers.
3. Repeat `k - 1` times:
   - Remove the largest element.
4. Return the next removed element.

---

## Dry Run

Input

```text
nums = [3,2,1,5,6,4]
k = 2
```

### Build Max Heap

```text
[6,5,4,3,2,1]
```

### Remove Largest Once

```text
poll() → 6
```

Remaining Heap

```text
[5,3,4,1,2]
```

### Next Poll

```text
poll() → 5
```

Answer

```text
5
```

---

## Java Solution (Max Heap)

```java
import java.util.*;

class Solution {
    public int findKthLargest(int[] nums, int k) {

        PriorityQueue<Integer> maxHeap =
                new PriorityQueue<>(Collections.reverseOrder());

        for (int num : nums) {
            maxHeap.offer(num);
        }

        for (int i = 1; i < k; i++) {
            maxHeap.poll();
        }

        return maxHeap.poll();
    }
}
```

### Complexity

| Operation | Complexity |
|-----------|------------|
| Build Heap | O(n log n) |
| Poll k times | O(k log n) |
| Space | O(n) |

---

# Approach 2: Min Heap (Optimal)

## Idea

Maintain a **Min Heap of size k**.

- Insert every element.
- If heap size exceeds `k`, remove the smallest element.
- After processing all elements, the root of the heap is the **kᵗʰ largest**.

---

## Why Does It Work?

The heap always stores only the **k largest elements** seen so far.

The smallest among these `k` elements is exactly the **kᵗʰ largest** element.

---

## Dry Run

Input

```text
nums = [3,2,1,5,6,4]
k = 2
```

| Element | Heap |
|---------|------|
|3|[3]|
|2|[2,3]|
|1|[1,2,3] → Remove 1 → [2,3]|
|5|[2,3,5] → Remove 2 → [3,5]|
|6|[3,5,6] → Remove 3 → [5,6]|
|4|[4,5,6] → Remove 4 → [5,6]|

Final Heap

```text
[5,6]
```

Root

```text
5
```

---

## Java Solution (Min Heap)

```java
import java.util.*;

class Solution {
    public int findKthLargest(int[] nums, int k) {

        PriorityQueue<Integer> minHeap = new PriorityQueue<>();

        for (int num : nums) {

            minHeap.offer(num);

            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }

        return minHeap.peek();
    }
}
```

### Complexity

| Operation | Complexity |
|-----------|------------|
| Insert n elements | O(n log k) |
| Space | O(k) |

---

# Max Heap vs Min Heap

| Feature | Max Heap | Min Heap |
|---------|----------|----------|
| Heap Type | Largest element at root | Smallest element at root |
| Idea | Remove largest `k-1` times | Keep only `k` largest elements |
| Answer | `poll()` | `peek()` |
| Time Complexity | O(n log n) | O(n log k) ✅ |
| Space Complexity | O(n) | O(k) ✅ |
| Interview Preference | Good | Best / Optimal |

---

# Which Approach Should You Use?

### Use Max Heap when:
- Learning heap basics.
- You need to repeatedly remove the largest element.
- Simplicity is more important than optimization.

### Use Min Heap when:
- Solving coding interviews.
- `k` is much smaller than `n`.
- You need the optimal solution.

---

# Key Concepts

- Heap
- Priority Queue
- Max Heap
- Min Heap
- Top K Elements
- Sorting
- Greedy

---

# Interview Tips

- Java's `PriorityQueue` is a **Min Heap** by default.
- Use `Collections.reverseOrder()` to create a **Max Heap**.
- `offer()` inserts an element.
- `poll()` removes the root element.
- `peek()` returns the root without removing it.
- **Max Heap:** Remove the largest element `k−1` times.
- **Min Heap:** Maintain only the `k` largest elements.
- The **Min Heap** solution (`O(n log k)`) is the optimal approach and is preferred in interviews.
