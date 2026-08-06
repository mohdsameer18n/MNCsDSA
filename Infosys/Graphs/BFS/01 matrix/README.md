# LeetCode 542 – 01 Matrix

## 📝 Problem Statement

Given an `m x n` binary matrix `mat`, return a matrix where each cell contains the distance to the **nearest 0**.

The distance between two adjacent cells is **1**, and movement is allowed only in four directions:

- Up
- Down
- Left
- Right

---

## Example 1

### Input

```text
mat =
[
 [0,0,0],
 [0,1,0],
 [0,0,0]
]
```

### Output

```text
[
 [0,0,0],
 [0,1,0],
 [0,0,0]
]
```

---

## Example 2

### Input

```text
mat =
[
 [0,0,0],
 [0,1,0],
 [1,1,1]
]
```

### Output

```text
[
 [0,0,0],
 [0,1,0],
 [1,2,1]
]
```

---

# 💡 Intuition

A brute-force solution would run **BFS from every cell containing `1`** until a `0` is found.

If there are `m × n` cells, this approach can take:

```
O((m × n)²)
```

which is too slow.

Instead, think in reverse.

Rather than searching **from every `1` to the nearest `0`**, start BFS **from every `0` simultaneously**.

Since BFS explores nodes level by level, the first time a cell is reached is guaranteed to be its shortest distance from any `0`.

This technique is called **Multi-Source BFS**.

---

# 🚀 Approach (Multi-Source BFS)

### Step 1

Create:

- Queue
- Distance matrix
- Visited matrix

### Step 2

Traverse the matrix.

If the current cell is `0`:

- Push it into the queue.
- Mark it as visited.

```java
q.offer(new int[]{i, j});
visited[i][j] = true;
```

### Step 3

Perform BFS.

For every popped cell:

- Visit all four neighbors.
- If the neighbor is inside the grid and unvisited:
    - Mark visited.
    - Set its distance.
    - Push it into the queue.

```java
dist[nr][nc] = dist[r][c] + 1;
```

### Step 4

Return the distance matrix.

---

# 🎯 Why Multi-Source BFS?

Suppose

```text
1 1 1
1 0 1
1 1 1
```

Instead of searching from every `1`,

start BFS from the center `0`.

### Level 0

```text
    0
```

### Level 1

```text
  1 0 1
```

### Level 2

```text
1 1 1
```

Distance matrix becomes

```text
2 1 2
1 0 1
2 1 2
```

The first visit always gives the shortest distance because BFS explores the graph level by level.

---

# 🧪 Dry Run

Input

```text
0 0 0
0 1 0
1 1 1
```

### Initial Queue

```text
(0,0)
(0,1)
(0,2)
(1,0)
(1,2)
```

### Distance Matrix

```text
0 0 0
0 0 0
0 0 0
```

### Visited

```text
T T T
T F T
F F F
```

---

### Pop (0,1)

Neighbor

```text
(1,1)
```

Distance

```text
dist[1][1] = 1
```

---

### Pop (1,0)

Neighbor

```text
(2,0)
```

Distance

```text
dist[2][0] = 1
```

---

### Pop (1,2)

Neighbor

```text
(2,2)
```

Distance

```text
dist[2][2] = 1
```

---

### Pop (1,1)

Neighbor

```text
(2,1)
```

Distance

```text
dist[2][1] = 2
```

Final Answer

```text
0 0 0
0 1 0
1 2 1
```

---

# ✅ Java Solution

```java
class Solution {

    int[][] dir = {
        {0,1},
        {1,0},
        {0,-1},
        {-1,0}
    };

    public int[][] updateMatrix(int[][] mat) {

        int m = mat.length;
        int n = mat[0].length;

        int[][] dist = new int[m][n];
        boolean[][] vis = new boolean[m][n];

        Queue<int[]> q = new LinkedList<>();

        // Add all 0s to the queue
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {

                if(mat[i][j] == 0) {
                    q.offer(new int[]{i, j});
                    vis[i][j] = true;
                }
            }
        }

        while(!q.isEmpty()) {

            int[] cur = q.poll();

            int r = cur[0];
            int c = cur[1];

            for(int[] d : dir) {

                int nr = r + d[0];
                int nc = c + d[1];

                if(nr >= 0 && nr < m &&
                   nc >= 0 && nc < n &&
                   !vis[nr][nc]) {

                    vis[nr][nc] = true;
                    dist[nr][nc] = dist[r][c] + 1;

                    q.offer(new int[]{nr, nc});
                }
            }
        }

        return dist;
    }
}
```

---

# ⏱ Complexity Analysis

### Time Complexity

```
O(m × n)
```

Each cell is visited exactly once.

### Space Complexity

```
O(m × n)
```

Used for:

- Queue
- Visited matrix
- Distance matrix

---

# ❌ Why Not DFS?

DFS explores one path completely before trying another.

It **does not guarantee the shortest distance** in an unweighted graph.

Example

```text
0 1 1
1 1 1
1 1 1
```

DFS may reach a cell through a longer path first.

BFS always explores the nearest cells first, ensuring the shortest distance.

---

# 📚 Pattern Recognition

Think **Multi-Source BFS** whenever you see:

- Nearest source
- Minimum distance
- Multiple starting points
- Unweighted graph
- Spread from multiple locations

---

# 🔥 Similar Problems

- LeetCode 994 – Rotting Oranges
- LeetCode 286 – Walls and Gates
- LeetCode 1162 – As Far from Land as Possible
- LeetCode 1765 – Map of Highest Peak

---

# 🧠 Key Takeaways

- Treat every `0` as a source.
- Push all sources into the queue before BFS starts.
- BFS explores nodes level by level.
- The first time a cell is visited, its distance is the shortest possible.
- Multi-Source BFS is the standard pattern for **nearest source** problems.
