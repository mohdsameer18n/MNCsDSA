# 994. Rotting Oranges

## Approach: Multi-Source BFS

### Code

```java
import java.util.*;

class Solution {
    public int orangesRotting(int[][] grid) {

        Queue<int[]> q = new ArrayDeque<>();
        int freshOranges = 0;
        int minute = 0;

        // Step 1: Store all rotten oranges
        // and count fresh oranges.
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {

                if (grid[i][j] == 2) {
                    q.offer(new int[]{i, j});
                } else if (grid[i][j] == 1) {
                    freshOranges++;
                }
            }
        }

        // No fresh oranges
        if (freshOranges == 0) {
            return 0;
        }

        int[][] direction = {
            {-1, 0},
            {1, 0},
            {0, -1},
            {0, 1}
        };

        // Multi-Source BFS
        while (!q.isEmpty()) {

            int size = q.size();
            boolean rotten = false;

            // One BFS level = One minute
            for (int i = 0; i < size; i++) {

                int[] curr = q.poll();
                int r = curr[0];
                int c = curr[1];

                for (int[] dir : direction) {

                    int nr = r + dir[0];
                    int nc = c + dir[1];

                    if (nr >= 0 &&
                        nr < grid.length &&
                        nc >= 0 &&
                        nc < grid[0].length &&
                        grid[nr][nc] == 1) {

                        grid[nr][nc] = 2;
                        freshOranges--;
                        rotten = true;

                        q.offer(new int[]{nr, nc});
                    }
                }
            }

            if (rotten) {
                minute++;
            }
        }

        return freshOranges == 0 ? minute : -1;
    }
}
```

---

# Dry Run

## Input

```text
grid =
[
 [2,1,1],
 [1,1,0],
 [0,1,1]
]
```

Initial Grid

```text
2 1 1
1 1 0
0 1 1
```

Fresh Oranges = **6**

Initial Queue

```text
[(0,0)]
```

---

## Minute 0 (Initial State)

Current Queue

```text
[(0,0)]
```

Current Grid

```text
2 1 1
1 1 0
0 1 1
```

Process `(0,0)`:

- Up → Invalid
- Down → `(1,0)` becomes rotten
- Left → Invalid
- Right → `(0,1)` becomes rotten

Updated Grid

```text
2 2 1
2 1 0
0 1 1
```

Queue for next minute

```text
[(1,0), (0,1)]
```

Fresh Oranges

```text
6 → 4
```

Minutes

```text
1
```

---

## Minute 1

Current Queue

```text
[(1,0), (0,1)]
```

Current Grid

```text
2 2 1
2 1 0
0 1 1
```

Process `(1,0)`

- `(1,1)` becomes rotten

Process `(0,1)`

- `(0,2)` becomes rotten

Updated Grid

```text
2 2 2
2 2 0
0 1 1
```

Queue

```text
[(1,1), (0,2)]
```

Fresh Oranges

```text
4 → 2
```

Minutes

```text
2
```

---

## Minute 2

Current Queue

```text
[(1,1), (0,2)]
```

Current Grid

```text
2 2 2
2 2 0
0 1 1
```

Process `(1,1)`

- `(2,1)` becomes rotten

Process `(0,2)`

- No new oranges

Updated Grid

```text
2 2 2
2 2 0
0 2 1
```

Queue

```text
[(2,1)]
```

Fresh Oranges

```text
2 → 1
```

Minutes

```text
3
```

---

## Minute 3

Current Queue

```text
[(2,1)]
```

Current Grid

```text
2 2 2
2 2 0
0 2 1
```

Process `(2,1)`

- `(2,2)` becomes rotten

Updated Grid

```text
2 2 2
2 2 0
0 2 2
```

Queue

```text
[(2,2)]
```

Fresh Oranges

```text
1 → 0
```

Minutes

```text
4
```

---

## Minute 4

Current Queue

```text
[(2,2)]
```

No fresh neighbours.

Queue becomes empty.

Since

```text
freshOranges == 0
```

Return

```text
4
```

---

# Visualization

### Initial

```text
2 1 1
1 1 0
0 1 1
```

↓

### After 1 minute

```text
2 2 1
2 1 0
0 1 1
```

↓

### After 2 minutes

```text
2 2 2
2 2 0
0 1 1
```

↓

### After 3 minutes

```text
2 2 2
2 2 0
0 2 1
```

↓

### After 4 minutes

```text
2 2 2
2 2 0
0 2 2
```

---

# Why `size = q.size()`?

Suppose the queue contains:

```text
[(1,0), (0,1)]
```

These oranges were already rotten **before this minute started**.

Only these oranges should spread the rot during the current minute.

Any newly rotten oranges are added to the queue:

```text
[(1,1), (0,2)]
```

but they **must wait until the next minute** to spread the infection.

Therefore:

```java
int size = q.size();
```

freezes the current level, ensuring that **one BFS level = one minute**.

---

# Complexity Analysis

### Time Complexity

- Scan the grid: **O(m × n)**
- BFS traversal: **O(m × n)**

Overall:

```text
O(m × n)
```

### Space Complexity

Queue stores at most all cells.

```text
O(m × n)
```

---

# Key Takeaways

- This is a **Multi-Source BFS** problem.
- Every initially rotten orange is added to the queue.
- **Each BFS level represents one minute**.
- `size = q.size()` ensures newly rotten oranges spread only in the **next minute**.
- Each fresh orange is visited exactly once.
- Return **-1** if any fresh orange remains after BFS.
- Return **0** if there are no fresh oranges initially.
