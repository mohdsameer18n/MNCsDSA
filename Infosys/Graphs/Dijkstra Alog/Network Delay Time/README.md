# 743. Network Delay Time

## Problem Statement

You are given:

- `n` nodes labeled from **1 to n**
- A directed weighted graph represented by `times`, where:

```text
times[i] = [u, v, w]
```

- `u` → source node
- `v` → destination node
- `w` → time required to travel from `u` to `v`

Initially, a signal is sent from node `k`.

Return the **minimum time** required for **all nodes** to receive the signal.

If some node cannot receive the signal, return **-1**.

---

## Example

### Input

```text
times = [[2,1,1],[2,3,1],[3,4,1]]

n = 4

k = 2
```

### Output

```text
2
```

### Explanation

```
        (1)
     1 <-----
             |
             |
             |1
             |
2 ---------->3
 \            |
  \           |1
   \          |
    ---------->4
       1
```

Shortest time from node **2**

| Node | Distance |
|------|----------|
|2|0|
|1|1|
|3|1|
|4|2|

Maximum shortest distance = **2**

---

# Intuition

We need to send a signal from one source node to every other node in the minimum possible time.

Since:

- Graph is **weighted**
- Edge weights are **non-negative**
- Need **shortest path from one source**

This is a classic **Dijkstra's Algorithm** problem.

---

# Approach

### Step 1

Build an adjacency list.

```text
u → (v, weight)
```

---

### Step 2

Initialize the shortest distance array.

```text
dist[i] = ∞
```

Source node

```text
dist[k] = 0
```

---

### Step 3

Use a **Min Heap (Priority Queue)**.

Store

```text
(distance, node)
```

The node having the smallest distance is always processed first.

---

### Step 4

Relax all adjacent edges.

If

```text
currentDistance + edgeWeight < dist[next]
```

then

```text
Update distance

Push into Priority Queue
```

---

### Step 5

If any node remains unreachable,

```text
return -1
```

Otherwise,

```text
return maximum distance
```

---

# Algorithm

```
Build Graph

Initialize distance[]

distance[source]=0

Insert source into Min Heap

while PQ not empty

    remove minimum distance node

    if entry is outdated
        continue

    relax all neighbours

Find maximum distance

If any node is unreachable

    return -1

Else

    return maximum distance
```

---

# Dry Run

Input

```text
times =

2 → 1 (1)

2 → 3 (1)

3 → 4 (1)

k = 2
```

---

Initial

```
Distance

1 = ∞

2 = 0

3 = ∞

4 = ∞
```

Priority Queue

```
[(0,2)]
```

---

## Step 1

Pop

```
(0,2)
```

Visit neighbors

```
1

distance = 1
```

Push

```
(1,1)
```

Visit

```
3

distance = 1
```

Push

```
(1,3)
```

Priority Queue

```
(1,1)

(1,3)
```

---

## Step 2

Pop

```
(1,1)
```

No outgoing edge.

Queue

```
(1,3)
```

---

## Step 3

Pop

```
(1,3)
```

Visit

```
4

distance = 2
```

Queue

```
(2,4)
```

---

## Step 4

Pop

```
(2,4)
```

No outgoing edges.

Finished.

Distance Array

```
Node

1 → 1

2 → 0

3 → 1

4 → 2
```

Answer

```
max = 2
```

---

# Why Priority Queue?

Suppose the graph is

```
1 ----10---->2

 \

  2

   \

    v

     3 ----1---->2
```

Initially,

```
Distance[2]=10

Distance[3]=2
```

If we process node **2** first (using a normal queue), we miss the shorter path.

A **Priority Queue** always processes the node with the **smallest known distance**, ensuring correctness for graphs with non-negative weights.

---

# Why Skip Outdated Entries?

Suppose the Priority Queue contains

```
(8,2)

(5,2)
```

Current shortest distance

```
dist[2]=5
```

When `(8,2)` is removed,

```
8 > 5
```

This entry is outdated.

Hence

```java
if (wt > dist[node])
    continue;
```

This optimization avoids unnecessary processing.

---

# Java Solution

```java
class Solution {

    public int networkDelayTime(int[][] times, int n, int k) {

        ArrayList<ArrayList<int[]>> graph = new ArrayList<>();

        for(int i = 0; i <= n; i++)
            graph.add(new ArrayList<>());

        // Build Graph
        for(int[] edge : times){

            int u = edge[0];
            int v = edge[1];
            int w = edge[2];

            graph.get(u).add(new int[]{v, w});
        }

        int[] dist = new int[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE);

        dist[k] = 0;

        PriorityQueue<int[]> pq =
                new PriorityQueue<>((a,b) -> a[0] - b[0]);

        pq.offer(new int[]{0, k});

        while(!pq.isEmpty()){

            int[] cur = pq.poll();

            int wt = cur[0];
            int node = cur[1];

            // Skip outdated entry
            if(wt > dist[node])
                continue;

            for(int[] next : graph.get(node)){

                int neighbor = next[0];
                int weight = next[1];

                int newDist = wt + weight;

                if(newDist < dist[neighbor]){

                    dist[neighbor] = newDist;

                    pq.offer(new int[]{newDist, neighbor});
                }
            }
        }

        int answer = 0;

        for(int i = 1; i <= n; i++){

            if(dist[i] == Integer.MAX_VALUE)
                return -1;

            answer = Math.max(answer, dist[i]);
        }

        return answer;
    }
}
```

---

# Complexity Analysis

### Time Complexity

Building graph

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

where:

- **V = Number of Nodes**
- **E = Number of Edges**

---

### Space Complexity

Adjacency List

```
O(V + E)
```

Distance Array

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

# Pattern Recognition

This problem belongs to:

- Graph
- Shortest Path
- Dijkstra's Algorithm
- Greedy
- Priority Queue (Min Heap)

---

# Key Observations

- Graph is **directed**.
- Edge weights are **positive**, making Dijkstra applicable.
- Use an adjacency list for efficient graph representation.
- Always process the node with the smallest current distance.
- Skip outdated entries using:

```java
if (wt > dist[node])
    continue;
```

- The answer is the **maximum shortest distance** from the source to all reachable nodes.
- If any node remains unreachable, return **-1**.

---

# Similar Problems

| Problem | Pattern |
|----------|---------|
| 743. Network Delay Time | Dijkstra |
| 1631. Path With Minimum Effort | Dijkstra |
| 778. Swim in Rising Water | Dijkstra |
| 1514. Path with Maximum Probability | Modified Dijkstra |
| 1976. Number of Ways to Arrive at Destination | Dijkstra + DP |
| 787. Cheapest Flights Within K Stops | Dijkstra / BFS |
| 2290. Minimum Obstacle Removal to Reach Corner | 0-1 BFS / Dijkstra |
| 2642. Design Graph With Shortest Path Calculator | Dijkstra |
| 505. The Maze II | Dijkstra |
| 1091. Shortest Path in Binary Matrix | BFS (Unit Weight Graph) |
