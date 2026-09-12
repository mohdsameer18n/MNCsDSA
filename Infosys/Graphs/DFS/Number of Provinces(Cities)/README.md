# LeetCode 547 — Number of Provinces

## Problem

You are given an `n x n` matrix `isConnected`, where:

* `isConnected[i][j] == 1` means city `i` is directly connected to city `j`.
* `isConnected[i][j] == 0` means there is no direct connection.

A **province** is a group of cities that are directly or indirectly connected.

Return the number of provinces.

---

## Graph Type

This problem represents an:

* **Undirected graph**
* **Unweighted graph**
* **Adjacency Matrix**
* We need to find the number of **Connected Components**

Example:

```text
isConnected = [
    [1, 1, 0],
    [1, 1, 0],
    [0, 0, 1]
]
```

Graph:

```text
    0
    |
    1

    2
```

There are two connected components:

```text
Province 1 = {0, 1}
Province 2 = {2}
```

Answer:

```text
2
```

---

## Important Concept

Do **not** treat the matrix as a normal 2D grid.

### Wrong approach

Moving in four directions:

```text
        up
        ↑
left ← cell → right
        ↓
       down
```

That approach is useful for problems involving:

* Islands
* Grid traversal
* Number of Islands
* Flood Fill

### Correct approach

Treat every **row as a city/node**.

For city `i`, check every city `j`:

```java
if (isConnected[i][j] == 1)
```

This means there is an edge between city `i` and city `j`.

---

# DFS Approach

We maintain a `visited[]` array.

```text
visited = [false, false, false]
```

Whenever we find an unvisited city:

1. Increment `count`.
2. Run DFS from that city.
3. DFS visits every city belonging to that province.
4. Continue searching for another unvisited city.

---

## Java Solution

```java
class Solution {

    public void dfs(int city, int[][] isConnected, boolean[] visited) {

        visited[city] = true;

        for (int nextCity = 0; nextCity < isConnected.length; nextCity++) {

            if (isConnected[city][nextCity] == 1
                    && !visited[nextCity]) {

                dfs(nextCity, isConnected, visited);
            }
        }
    }

    public int findCircleNum(int[][] isConnected) {

        int n = isConnected.length;

        boolean[] visited = new boolean[n];

        int count = 0;

        for (int city = 0; city < n; city++) {

            if (!visited[city]) {

                count++;

                dfs(city, isConnected, visited);
            }
        }

        return count;
    }
}
```

---

# Dry Run

Input:

```text
isConnected = [
    [1, 1, 0],
    [1, 1, 0],
    [0, 0, 1]
]
```

Initially:

```text
visited = [false, false, false]
count = 0
```

### City 0

City 0 is unvisited.

```text
count = 1
```

Run:

```java
dfs(0, ...)
```

City 0 is connected to city 1.

```text
0 → 1
```

DFS visits city 1.

```text
visited = [true, true, false]
```

Province found:

```text
{0, 1}
```

---

### City 1

Already visited:

```text
visited[1] = true
```

Skip.

---

### City 2

City 2 is unvisited.

```text
count = 2
```

Run:

```java
dfs(2, ...)
```

City 2 has no connection to cities 0 or 1.

```text
visited = [true, true, true]
```

Province found:

```text
{2}
```

---

## Final Answer

```text
count = 2
```

Therefore:

```text
Output: 2
```

---

# Complexity

Let `n` be the number of cities.

### Time Complexity

```text
O(n²)
```

For every city, we potentially check all `n` cities.

### Space Complexity

```text
O(n)
```

For:

* `visited[]`
* DFS recursion stack

---

# Pattern to Remember

When you see:

```text
isConnected[i][j]
```

and it represents:

```text
city i ↔ city j
```

think:

```text
Adjacency Matrix
       ↓
Undirected Graph
       ↓
Connected Components
       ↓
DFS / BFS
```

When you see an actual grid such as:

```text
1 0 1
1 1 0
0 1 1
```

where cells are connected spatially, think:

```text
Grid
 ↓
4-direction / 8-direction DFS or BFS
```

## Key Difference

**LeetCode 547:**

```text
city → city
```

Use graph DFS/BFS.

**Number of Islands:**

```text
cell → neighboring cell
```

Use grid DFS/BFS.
