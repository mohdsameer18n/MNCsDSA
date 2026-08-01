# Rotting Oranges

## Approach: Multi-Source BFS

### Code

```java
import java.util.*;

class Solution {
    public int orangesRotting(int[][] grid) {

        Queue<int[]> q = new ArrayDeque<>();
        int freshOranges = 0;
        int minute = 0;

        // Step 1: Add all rotten oranges to the queue
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

        // If there are no fresh oranges
        if (freshOranges == 0) {
            return 0;
        }

        // Four possible directions
        int[][] direction = {
            {-1, 0},
            {1, 0},
            {0, -1},
            {0, 1}
        };

        // Step 2: Perform Multi-Source BFS
        while (!q.isEmpty()) {

            int size = q.size();

            // Tracks whether any fresh orange
            // became rotten during this minute.
            boolean rotten = false;

            // Process one BFS level (one minute)
            for (int i = 0; i < size; i++) {

                int[] curr = q.poll();

                int r = curr[0];
                int c = curr[1];

                // Visit all 4 neighbours
                for (int[] dir : direction) {

                    int nr = r + dir[0];
                    int nc = c + dir[1];

                    // If neighbour is inside grid
                    // and is a fresh orange
                    if (nr >= 0 &&
                        nr < grid.length &&
                        nc >= 0 &&
                        nc < grid[0].length &&
                        grid[nr][nc] == 1) {

                        // Rot the fresh orange
                        grid[nr][nc] = 2;

                        // One less fresh orange
                        freshOranges--;

                        // New rotten orange
                        q.offer(new int[]{nr, nc});

                        rotten = true;
                    }
                }
            }

            // Increase minute only if
            // at least one orange rotted.
            if (rotten) {
                minute++;
            }
        }

        // If fresh oranges remain,
        // they were unreachable.
        return freshOranges == 0 ? minute : -1;
    }
}
```

---

# Code Explanation

### Queue Initialization

```java
Queue<int[]> q = new ArrayDeque<>();
```

Stores the positions of all rotten oranges.

Each element is:

```text
[row, col]
```

Example:

```text
[(0,0), (2,1), (3,4)]
```

---

### Count Fresh Oranges

```java
int freshOranges = 0;
```

Counts how many fresh oranges need to become rotten.

---

### Store All Rotten Oranges

```java
if(grid[i][j] == 2){
    q.offer(new int[]{i, j});
}
```

Every rotten orange is inserted into the queue because they all spread simultaneously.

---

### Early Return

```java
if(freshOranges == 0){
    return 0;
}
```

If there are no fresh oranges, no time is needed.

---

### Directions Array

```java
int[][] direction = {
    {-1,0},
    {1,0},
    {0,-1},
    {0,1}
};
```

Represents:

```text
Up
Down
Left
Right
```

---

### BFS Starts

```java
while(!q.isEmpty())
```

Runs until no rotten oranges are left to spread the infection.

---

### Process One Minute

```java
int size = q.size();
```

The current queue contains oranges that are rotten **at the beginning of this minute**.

We process only these oranges.

---

### Check if Anything Rotted

```java
boolean rotten = false;
```

Used to determine whether to increase the minute count.

If no fresh orange becomes rotten during this level, we don't increment the time.

---

### Remove Current Rotten Orange

```java
int[] curr = q.poll();
```

Take one rotten orange from the queue.

---

### Visit Four Neighbours

```java
for(int[] dir : direction)
```

Generate neighbour coordinates:

```java
int nr = r + dir[0];
int nc = c + dir[1];
```

---

### Check Valid Fresh Orange

```java
if(nr >= 0 &&
   nr < grid.length &&
   nc >= 0 &&
   nc < grid[0].length &&
   grid[nr][nc] == 1)
```

Ensures:

- Inside the grid
- Fresh orange

---

### Rot the Orange

```java
grid[nr][nc] = 2;
```

Mark it as rotten to avoid processing it again.

---

### Update Fresh Count

```java
freshOranges--;
```

One fresh orange has now become rotten.

---

### Add to Queue

```java
q.offer(new int[]{nr, nc});
```

This orange will spread the rot during the **next minute**.

---

### Mark That Something Rotted

```java
rotten = true;
```

Used later to increase the minute count.

---

### Increase Minute

```java
if(rotten){
    minute++;
}
```

Increase time only if at least one fresh orange became rotten during this BFS level.

---

### Final Answer

```java
return freshOranges == 0 ? minute : -1;
```

- Return `minute` if every fresh orange became rotten.
- Return `-1` if some fresh oranges could never be reached.

---

# Complexity Analysis

### Time Complexity

- Scan grid: **O(m × n)**
- BFS traversal: **O(m × n)**

**Overall:**

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
- All rotten oranges are added to the queue initially.
- One BFS level represents **one minute**.
- Every fresh orange is processed only once.
- If any fresh orange remains after BFS, return **-1**.
- If there are no fresh oranges initially, return **0**.
