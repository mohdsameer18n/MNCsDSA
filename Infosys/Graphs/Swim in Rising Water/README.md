# 778. Swim in Rising Water

## Problem

You are given an `n x n` grid where each cell contains a unique elevation.

- At time `t`, the water level is `t`.
- You can move in **4 directions** (up, down, left, right).
- You can enter a cell only if its elevation is **less than or equal to** the current water level.

Return the **minimum time** required to reach the bottom-right cell from the top-left cell.

---

## Example

### Input

```text
0 2
1 3
```

### Output

```text
3
```

Explanation:

At time 0

```text
0
```

At time 1

```text
0 → 1
```

At time 2

```text
0 → 2
```

At time 3

```text
0 → 1 → 3
```

Destination becomes reachable.

---

# Intuition

This is **NOT** a normal BFS problem.

In BFS every move costs the same.

Here every cell has a different elevation.

Suppose the path is

```text
0 → 1 → 5 → 2
```

You don't pay

```text
0+1+5+2
```

Instead, you must wait until the **highest elevation** on the path is underwater.

Required water level

```text
max(0,1,5,2)=5
```

Our goal is

> Find the path whose **maximum elevation is minimum**.

---

# Why Dijkstra?

Treat every cell as a graph node.

Instead of storing

```text
Distance
```

store

```text
Maximum elevation seen so far
```

Priority Queue always removes the path requiring the **smallest water level**.

---

# Algorithm

1. Create a Priority Queue.
2. Store

```text
(time,row,col)
```

3. Start from

```text
(grid[0][0],0,0)
```

4. Remove the smallest time from Priority Queue.
5. If destination is reached, return time.
6. Visit all 4 neighbors.
7. New required time

```java
Math.max(currentTime, grid[nr][nc])
```

8. Push into Priority Queue.

---

# Dry Run

## Input

```text
0 2
1 3
```

Initially

```text
Priority Queue

(0,0,0)

(time,row,col)
```

---

### Step 1

Remove

```text
(0,0,0)
```

Current Time

```text
0
```

Neighbors

```text
Right = 2

Down = 1
```

New Time

```text
Right

max(0,2)=2

Down

max(0,1)=1
```

Push

```text
(2,0,1)

(1,1,0)
```

Priority Queue

```text
Top

(1,1,0)

↓

(2,0,1)
```

---

### Step 2

Remove

```text
(1,1,0)
```

Current Time

```text
1
```

Neighbor

```text
3
```

New Time

```text
max(1,3)=3
```

Push

```text
(3,1,1)
```

Priority Queue

```text
Top

(2,0,1)

↓

(3,1,1)
```

---

### Step 3

Remove

```text
(2,0,1)
```

Neighbor

```text
3
```

New Time

```text
max(2,3)=3
```

Push

```text
(3,1,1)
```

---

### Step 4

Remove

```text
(3,1,1)
```

This is the destination.

Return

```text
3
```

---

# Why Math.max()?

Suppose current path needs water level

```text
7
```

Next cell is

```text
5
```

Then

```java
Math.max(7,5)=7
```

because the highest elevation is still 7.

Suppose next cell is

```text
10
```

Then

```java
Math.max(7,10)=10
```

Now you must wait until water reaches 10.

---

# Why Priority Queue?

Priority Queue always removes the path requiring the **least water level**.

Example

Insert

```text
(8,2,3)

(2,0,1)

(5,1,0)
```

Priority Queue

```text
Top

(2,0,1)

↓

(5,1,0)

↓

(8,2,3)
```

The smallest water level is always explored first.

---

# Boundary Check

```java
if(nr < 0 || nc < 0 || nr >= n || nc >= n || visited[nr][nc])
    continue;
```

Meaning

- `nr < 0` → moved above the first row.
- `nc < 0` → moved left of the first column.
- `nr >= n` → moved below the last row.
- `nc >= n` → moved right of the last column.
- `visited[nr][nc]` → already processed.

Skip invalid neighbors.

---

# Time Complexity

Each cell is inserted into the Priority Queue.

Priority Queue operations take

```text
O(log(n²))
```

Total

```text
O(n² log n)
```

---

# Space Complexity

```text
Visited Array

O(n²)

Priority Queue

O(n²)
```

Overall

```text
O(n²)
```

---

# Java Solution

```java
import java.util.*;

class Solution {

    public int swimInWater(int[][] grid) {

        int n = grid.length;

        int[][] directions = {
            {1,0},{-1,0},
            {0,1},{0,-1}
        };

        PriorityQueue<int[]> pq =
                new PriorityQueue<>((a,b)->a[0]-b[0]);

        boolean[][] visited = new boolean[n][n];

        pq.offer(new int[]{grid[0][0],0,0});

        while(!pq.isEmpty()){

            int[] curr = pq.poll();

            int time = curr[0];
            int row = curr[1];
            int col = curr[2];

            if(visited[row][col])
                continue;

            visited[row][col]=true;

            if(row==n-1 && col==n-1)
                return time;

            for(int[] dir: directions){

                int nr=row+dir[0];
                int nc=col+dir[1];

                if(nr<0 || nc<0 || nr>=n || nc>=n || visited[nr][nc])
                    continue;

                int newTime=Math.max(time,grid[nr][nc]);

                pq.offer(new int[]{newTime,nr,nc});
            }
        }

        return -1;
    }
}
```

---

# Pattern Recognition

When you see:

- Grid
- Minimum time
- Different movement costs
- Minimum maximum value

Think:

```text
Weighted Graph

↓

Dijkstra

↓

Priority Queue
```

This same pattern appears in:

- Swim in Rising Water
- Network Delay Time
- Path With Minimum Effort
- Minimum Cost Path
```
