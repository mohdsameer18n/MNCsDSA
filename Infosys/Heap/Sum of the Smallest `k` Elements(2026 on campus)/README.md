# Sum of the Smallest `k` Elements (Minimum Element as Negative)

## Problem Statement

Given an array of integers `arr` and an integer `k`:

1. Insert all array elements into a **Min Heap**.
2. Remove the smallest `k` elements.
3. Among these `k` elements:
   - Treat the **smallest element** as **negative**.
   - Add the remaining `k - 1` elements normally.
4. Return the final sum.

---

## Example

### Input

```text
arr = [8, 3, 5, 1, 7]
k = 3
```

### Min Heap Order

```text
1, 3, 5, 7, 8
```

### Selected Elements

```text
[1, 3, 5]
```

Treat the smallest element as negative:

```text
-1 + 3 + 5 = 7
```

### Output

```text
7
```

---

## Approach

1. Create a **Min Heap** using `PriorityQueue`.
2. Insert all array elements into the heap.
3. Remove the smallest element and convert it to negative.
4. Remove the remaining `k - 1` smallest elements and add them to the answer.
5. Print the final result.

---

## Algorithm

1. Read the array and value of `k`.
2. Insert every array element into a Min Heap.
3. Initialize `answer = -poll()`.
4. Repeat `k - 1` times:
   - Remove the next smallest element.
   - Add it to `answer`.
5. Print `answer`.

---

## Java Code

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();
        int[] arr = new int[n];

        for (int i = 0; i < n; i++) {
            arr[i] = scan.nextInt();
        }

        int k = scan.nextInt();

        PriorityQueue<Integer> minHeap = new PriorityQueue<>();

        for (int num : arr) {
            minHeap.offer(num);
        }

        int ans = 0;

        if (k > 0) {
            ans = -minHeap.poll();

            for (int i = 1; i < k; i++) {
                ans += minHeap.poll();
            }
        }

        System.out.println(ans);
    }
}
```

---

## Dry Run

### Input

```text
arr = [8, 3, 5, 1, 7]
k = 3
```

### Step 1

Min Heap:

```text
[1, 3, 5, 7, 8]
```

### Step 2

Remove the smallest element:

```text
1
```

Take it as negative:

```text
answer = -1
```

### Step 3

Remove next smallest:

```text
3
answer = -1 + 3 = 2
```

### Step 4

Remove next smallest:

```text
5
answer = 2 + 5 = 7
```

### Final Output

```text
7
```

---

## Time Complexity

| Operation | Complexity |
|-----------|------------|
| Insert all elements into heap | O(n log n) |
| Remove `k` elements | O(k log n) |
| **Overall** | **O(n log n + k log n)** |

---

## Space Complexity

```text
O(n)
```

The Min Heap stores all `n` elements.

---

## Key Concepts

- **PriorityQueue** in Java implements a **Min Heap** by default.
- `poll()` removes and returns the smallest element.
- `offer()` inserts an element into the heap.
- The smallest selected element is converted to negative using the unary minus (`-`) operator.
