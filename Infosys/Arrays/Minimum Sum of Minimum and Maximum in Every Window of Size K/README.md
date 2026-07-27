# Minimum Sum of Minimum and Maximum in Every Window of Size K

## Problem Statement

Given an array `arr[]` of size `N` and an integer `K`, consider every contiguous subarray (window) of size `K`.

For each window:

- Find the **minimum** element.
- Find the **maximum** element.
- Compute their sum (`min + max`).

Return the **minimum** value of `(min + max)` among all windows.

---

## Example

### Input

```text
arr = [4, 2, 7, 1, 5]
k = 3
```

### Windows

| Window | Minimum | Maximum | Sum |
|--------|---------|---------|-----|
| [4, 2, 7] | 2 | 7 | 9 |
| [2, 7, 1] | 1 | 7 | 8 |
| [7, 1, 5] | 1 | 7 | 8 |

### Output

```text
8
```

---

# Approach 1: Brute Force

## Idea

- Traverse every window of size `k`.
- For each window:
  - Find the minimum element.
  - Find the maximum element.
  - Compute `min + max`.
- Keep track of the minimum sum among all windows.

---

## Algorithm

1. Iterate through every possible window.
2. For each window:
   - Initialize `min = Integer.MAX_VALUE`
   - Initialize `max = Integer.MIN_VALUE`
3. Traverse the current window.
4. Update minimum and maximum.
5. Compute `min + max`.
6. Update the answer.

---

## Java Code

```java
import java.util.*;

public class Main {
    public static void main(String args[]) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

        int[] arr = new int[n];

        for (int i = 0; i < n; i++) {
            arr[i] = scan.nextInt();
        }

        int k = scan.nextInt();

        int ans = Integer.MAX_VALUE;

        for (int i = 0; i <= n - k; i++) {

            int min = Integer.MAX_VALUE;
            int max = Integer.MIN_VALUE;

            for (int j = i; j < i + k; j++) {
                min = Math.min(min, arr[j]);
                max = Math.max(max, arr[j]);
            }

            ans = Math.min(ans, min + max);
        }

        System.out.println(ans);
    }
}
```

---

## Dry Run

```
arr = [4,2,7,1,5]
k = 3

Window 1
[4,2,7]
min = 2
max = 7
sum = 9

Answer = 9

------------------

Window 2
[2,7,1]
min = 1
max = 7
sum = 8

Answer = min(9,8)=8

------------------

Window 3
[7,1,5]
min = 1
max = 7
sum = 8

Answer = min(8,8)=8
```

---

## Complexity Analysis

- **Time Complexity:** `O(N × K)`
- **Space Complexity:** `O(1)`

---

# Approach 2: Optimal (Using Two Deques)

## Idea

Maintain two deques:

- **Min Deque** → Stores indices in increasing order of values.
- **Max Deque** → Stores indices in decreasing order of values.

The front of:

- Min deque → Minimum element of current window.
- Max deque → Maximum element of current window.

Each element is inserted and removed at most once.

---

## Algorithm

For every index:

1. Remove indices outside the current window.
2. Maintain increasing order in the min deque.
3. Maintain decreasing order in the max deque.
4. Once a complete window is formed:
   - Read minimum from the front of the min deque.
   - Read maximum from the front of the max deque.
   - Update the answer.

---

## Java Code

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

        int[] arr = new int[n];

        for (int i = 0; i < n; i++)
            arr[i] = scan.nextInt();

        int k = scan.nextInt();

        Deque<Integer> minDeque = new LinkedList<>();
        Deque<Integer> maxDeque = new LinkedList<>();

        int ans = Integer.MAX_VALUE;

        for (int i = 0; i < n; i++) {

            while (!minDeque.isEmpty() && minDeque.peekFirst() <= i - k)
                minDeque.pollFirst();

            while (!maxDeque.isEmpty() && maxDeque.peekFirst() <= i - k)
                maxDeque.pollFirst();

            while (!minDeque.isEmpty() &&
                   arr[minDeque.peekLast()] >= arr[i])
                minDeque.pollLast();

            minDeque.offerLast(i);

            while (!maxDeque.isEmpty() &&
                   arr[maxDeque.peekLast()] <= arr[i])
                maxDeque.pollLast();

            maxDeque.offerLast(i);

            if (i >= k - 1) {

                int min = arr[minDeque.peekFirst()];
                int max = arr[maxDeque.peekFirst()];

                ans = Math.min(ans, min + max);
            }
        }

        System.out.println(ans);
    }
}
```

---

## Dry Run

```
arr = [4,2,7,1,5]
k = 3

Window 1
Min = 2
Max = 7
Sum = 9

Window 2
Min = 1
Max = 7
Sum = 8

Window 3
Min = 1
Max = 7
Sum = 8

Final Answer = 8
```

---

## Complexity Analysis

### Brute Force

- **Time Complexity:** `O(N × K)`
- **Space Complexity:** `O(1)`

### Optimal (Deque)

- **Time Complexity:** `O(N)`
- **Space Complexity:** `O(K)`

---

# Comparison

| Feature | Brute Force | Deque (Optimal) |
|----------|------------|-----------------|
| Time Complexity | `O(N × K)` | `O(N)` |
| Space Complexity | `O(1)` | `O(K)` |
| Easy to Understand | ✅ Yes | Moderate |
| Suitable for Large Input | ❌ No | ✅ Yes |
| Interview Recommended | ❌ | ✅ |

---

# Key Points

- Sliding Window problem.
- Need both **minimum** and **maximum** for every window.
- Brute force scans every window completely.
- Deque stores indices efficiently.
- Each element is inserted and removed only once.
- The deque approach is the optimal interview solution with **O(N)** time complexity.
