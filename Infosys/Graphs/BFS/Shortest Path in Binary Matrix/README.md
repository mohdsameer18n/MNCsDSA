# Shortest Path in Binary Matrix

## Problem Statement

Given an `n x n` binary matrix `grid`:

- `0` represents an empty cell.
- `1` represents a blocked cell.

Return the length of the **shortest clear path** from the top-left corner `(0,0)` to the bottom-right corner `(n-1,n-1)`.

A clear path:

- Starts at `(0,0)`
- Ends at `(n-1,n-1)`
- Visits only cells containing `0`
- Can move in **8 directions**:
  - Up
  - Down
  - Left
  - Right
  - Top-Left
  - Top-Right
  - Bottom-Left
  - Bottom-Right

If no such path exists, return `-1`.

---

# Example

### Input

```text
grid =
[
 [0,1],
 [1,0]
]
```

### Output

```text
2
```

### Explanation

```
(0,0)
   ↘
   (1,1)
```

Path length = **2**

---

# Intuition

Since every move costs exactly **1**, we need the **minimum number of moves**.

This is a classic **Shortest Path in an Unweighted Graph** problem.

BFS explores cells level by level, so the first time we reach the destination is guaranteed to be the shortest path.

---

# Approach

1. If the source or destination is blocked, return `-1`.
2. Create a distance matrix initialized with `-1`.
3. Start BFS from `(0,0)`.
4. Explore all **8 directions**.
5. Skip:
   - Out of bounds cells
   - Blocked cells (`1`)
   - Already visited cells
6. Update the distance of each newly visited cell.
7. Return the distance at `(n-1,n-1)`.

---

# Algorithm

1. Check whether the source or destination is blocked.
2. Initialize `dist[][]` with `-1`.
3. Set

```text
dist[0][0] = 1
```

4. Push `(0,0)` into the queue.
5. While the queue is not empty:
   - Pop a cell.
   - Visit all 8 neighbours.
   - If valid and unvisited:
     - Update distance.
     - Push into queue.
6. Return `dist[n-1][n-1]`.

---

# Dry Run

Input

```text
0 1
1 0
```

Initial

```
Queue

(0,0)

dist

1 -1
-1 -1
```

---

Process (0,0)

Valid neighbour

```
(1,1)
```

Update

```
dist

1 -1
-1 2
```

Queue

```
(1,1)
```

---

Process (1,1)

Destination reached.

Answer

```
2
```

---

# Why BFS?

Every edge has equal weight.

```
Cost = 1
```

BFS always visits cells in increasing order of distance.

Therefore, the first time we reach a cell is its shortest distance.

---

# Correctness

- Every valid cell is visited at most once.
- BFS processes cells in increasing distance order.
- The first distance assigned to a cell is its minimum distance from the source.
- Hence, the destination's distance is the length of the shortest clear path.

Therefore, the algorithm is correct.

---

# Complexity Analysis

### Time Complexity

Each cell is processed at most once.

```
O(N²)
```

---

### Space Complexity

Distance Matrix

```
O(N²)
```

Queue

```
O(N²)
```

Overall

```
O(N²)
```

---

# Java Code

```java
import java.util.*;

class Solution {

    public int shortestPathBinaryMatrix(int[][] grid) {

        int[][] directions = {
            {1,0}, {0,1},
            {-1,0}, {0,-1},
            {1,1}, {-1,-1},
            {-1,1}, {1,-1}
        };

        int m = grid.length;
        int n = grid[0].length;

        if (grid[0][0] == 1 || grid[m-1][n-1] == 1) {
            return -1;
        }

        int[][] dist = new int[m][n];

        for (int i = 0; i < m; i++) {
            Arrays.fill(dist[i], -1);
        }

        dist[0][0] = 1;

        Queue<int[]> q = new ArrayDeque<>();
        q.offer(new int[]{0, 0});

        while (!q.isEmpty()) {

            int[] curr = q.poll();

            int r = curr[0];
            int c = curr[1];

            // Optional optimization
            if (r == m - 1 && c == n - 1) {
                return dist[r][c];
            }

            for (int[] dir : directions) {

                int nr = r + dir[0];
                int nc = c + dir[1];

                if (nr < 0 || nr >= m ||
                    nc < 0 || nc >= n ||
                    grid[nr][nc] == 1) {
                    continue;
                }

                if (dist[nr][nc] != -1) {
                    continue;
                }

                dist[nr][nc] = dist[r][c] + 1;
                q.offer(new int[]{nr, nc});
            }
        }

        return dist[m - 1][n - 1];
    }
}
```

---

# Pattern Recognition

This problem belongs to the **Shortest Path in an Unweighted Grid** pattern.

### Clues

- Minimum path
- Equal edge weights
- Grid traversal
- 8-direction movement

Think of:

```
BFS
```

---

# Similar Problems

- LeetCode 542 – 01 Matrix
- LeetCode 994 – Rotting Oranges
- LeetCode 286 – Walls and Gates
- LeetCode 1926 – Nearest Exit from Entrance in Maze
- LeetCode 1293 – Shortest Path in a Grid with Obstacles Elimination

---

# Key Takeaways

- Use **BFS** for shortest paths in unweighted grids.
- Mark cells as visited when they are added to the queue.
- Since all moves have equal cost, BFS guarantees the shortest path.
- In this problem, movement is allowed in **8 directions**, unlike many grid problems that allow only 4.
