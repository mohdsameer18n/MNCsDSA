# LeetCode 1976 - Number of Ways to Arrive at Destination

## Problem Statement

You are given an undirected weighted graph with `n` intersections numbered from `0` to `n-1`.

Each road is represented as:

```text
[u, v, time]
```

- `u` = starting intersection
- `v` = ending intersection
- `time` = travel time

Return the **number of different shortest paths** from node `0` to node `n-1`.

Since the answer can be very large, return it modulo **1,000,000,007**.

---

## Example

**Input**

```text
n = 7

roads =

[
[0,6,7],
[0,1,2],
[1,2,3],
[1,3,3],
[6,3,3],
[3,5,1],
[6,5,1],
[2,5,1],
[0,4,5],
[4,6,2]
]
```

**Output**

```text
4
```

**Explanation**

The shortest distance from node `0` to node `6` is **7**.

There are **4** shortest paths:

```
0 → 6
0 → 4 → 6
0 → 1 → 2 → 5 → 6
0 → 1 → 3 → 5 → 6
```

---

# Approach

This problem is solved using **Dijkstra's Algorithm** along with **Dynamic Programming (Path Counting)**.

We maintain two arrays:

- `dist[i]` → Shortest distance from source to node `i`
- `ways[i]` → Number of shortest paths to reach node `i`

Initially,

```text
dist[0] = 0
ways[0] = 1
```

All other distances are initialized to infinity.

---

# Key Observation

While relaxing an edge `(node → next)`:

## Case 1 : Shorter Path Found

If

```text
newDistance < dist[next]
```

we found a better path.

Update

```java
dist[next] = newDistance;
ways[next] = ways[node];
```

Since every shortest path to `next` now comes through `node`.

---

## Case 2 : Another Shortest Path Found

If

```text
newDistance == dist[next]
```

then another shortest path exists.

Update

```java
ways[next] = (ways[next] + ways[node]) % MOD;
```

We keep the same distance but increase the number of shortest paths.

---

# Algorithm

1. Build an adjacency list.
2. Initialize

```text
dist[] = INF
ways[] = 0
```

3. Set

```text
dist[0] = 0
ways[0] = 1
```

4. Use a Priority Queue (Min Heap).

5. While the queue is not empty:

   - Pop the node having minimum distance.
   - Skip if it is an outdated entry.
   - Visit all neighbours.
   - Relax every edge.

6. Return

```text
ways[n-1]
```

---

# Dry Run

Initial

```text
dist = [0,∞,∞,∞,∞,∞,∞]

ways = [1,0,0,0,0,0,0]
```

---

Process node **0**

```text
dist = [0,2,∞,∞,5,∞,7]

ways = [1,1,0,0,1,0,1]
```

---

Process node **1**

```text
dist = [0,2,5,5,5,∞,7]

ways = [1,1,1,1,1,0,1]
```

---

Process node **4**

```
0 → 4 → 6

Distance = 7

Equal shortest distance
```

```text
ways[6] = 2
```

---

Process node **2**

```
0 → 1 → 2 → 5
```

```text
dist[5] = 6

ways[5] = 1
```

---

Process node **3**

```
0 → 1 → 3 → 5

Distance = 6

Equal shortest distance
```

```text
ways[5] = 2
```

---

Process node **5**

```
0 → 1 → 2 → 5 → 6

0 → 1 → 3 → 5 → 6
```

Both have distance **7**.

```text
ways[6] = 4
```

Final

```text
dist

[0,2,5,5,5,6,7]

ways

[1,1,1,1,1,2,4]
```

Answer

```text
4
```

---

# Correctness

For every node:

- `dist[i]` always stores the minimum distance discovered so far.
- `ways[i]` stores the number of shortest paths that produce `dist[i]`.

Whenever a shorter path is found, we replace both the distance and path count.

Whenever another shortest path is found, we simply add its contribution.

Since Dijkstra processes nodes in increasing order of distance, every shortest path is counted exactly once.

Therefore, the algorithm correctly returns the number of shortest paths.

---

# Complexity Analysis

### Time Complexity

Building Graph

```
O(E)
```

Dijkstra

```
O((V + E) log V)
```

Overall

```
O((V + E) log V)
```

---

### Space Complexity

Adjacency List

```
O(E)
```

Distance Array

```
O(V)
```

Ways Array

```
O(V)
```

Priority Queue

```
O(V)
```

Overall

```
O(V + E)
```

---

# Java Code

```java
class Solution {

    static final int MOD = 1_000_000_007;

    public int countPaths(int n, int[][] roads) {

        ArrayList<ArrayList<int[]>> graph = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }

        for (int[] road : roads) {

            int u = road[0];
            int v = road[1];
            int wt = road[2];

            graph.get(u).add(new int[]{v, wt});
            graph.get(v).add(new int[]{u, wt});
        }

        PriorityQueue<long[]> pq =
                new PriorityQueue<>((a, b) -> Long.compare(a[1], b[1]));

        long[] dist = new long[n];
        Arrays.fill(dist, Long.MAX_VALUE);

        int[] ways = new int[n];

        dist[0] = 0;
        ways[0] = 1;

        pq.offer(new long[]{0, 0});

        while (!pq.isEmpty()) {

            long[] curr = pq.poll();

            int node = (int) curr[0];
            long d = curr[1];

            if (d > dist[node]) continue;

            for (int[] nei : graph.get(node)) {

                int next = nei[0];
                int wt = nei[1];

                if (d + wt < dist[next]) {

                    dist[next] = d + wt;
                    ways[next] = ways[node];

                    pq.offer(new long[]{next, dist[next]});
                }

                else if (d + wt == dist[next]) {

                    ways[next] = (ways[next] + ways[node]) % MOD;
                }
            }
        }

        return ways[n - 1];
    }
}
```

---

# Pattern

✅ Graph

✅ Positive Edge Weights

✅ Shortest Distance

✅ Number of Shortest Paths

**Pattern:**

```
Dijkstra + DP (Shortest Path Counting)
```

**Key Arrays**

```text
dist[]  → Minimum distance

ways[]  → Number of shortest paths
```
