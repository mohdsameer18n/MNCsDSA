# Number of Operations to Make Network Connected

## 📝 Problem Statement

There are `n` computers numbered from `0` to `n - 1`.

You are given a list of existing network cables where:

```text
connections[i] = [a, b]
```

means there is a cable connecting computer `a` and computer `b`.

You can remove any existing cable and reconnect it between any two disconnected computers.

Return the **minimum number of operations** required to connect all computers.

If it is impossible, return **-1**.

---

## Example 1

### Input

```text
n = 4

connections =
[
 [0,1],
 [0,2],
 [1,2]
]
```

### Output

```text
1
```

### Explanation

Initially

```text
      0
     / \
    1---2

    3
```

The edge

```text
1 ---- 2
```

forms a cycle.

It is an **extra cable** because removing it does not disconnect computers `0`, `1`, and `2`.

Remove this cable and connect

```text
1 ---- 3
```

Now all computers become connected.

---

## Example 2

### Input

```text
n = 6

connections =
[
 [0,1],
 [0,2],
 [0,3],
 [1,2],
 [1,3]
]
```

### Output

```text
2
```

---

# 💡 Intuition

A network is fully connected if every computer belongs to the same connected component.

This problem asks us to determine:

* How many connected components exist?
* Do we have enough cables to connect them?

The best data structure for dynamically merging components is the **Disjoint Set Union (DSU)**, also known as **Union-Find**.

---

# 🔍 Key Observation

A connected graph with `n` nodes always needs at least

```text
n - 1
```

edges.

Therefore,

```java
if(connections.length < n - 1)
    return -1;
```

If there are fewer than `n - 1` cables, connecting all computers is impossible.

---

# 🚀 Approach (Union-Find)

### Step 1

If

```text
connections < n - 1
```

return

```text
-1
```

---

### Step 2

Initialize every computer as its own parent.

```text
0 1 2 3 4
```

Each computer is its own component.

---

### Step 3

Process every cable.

Find the root parent of both endpoints.

```java
u = find(a);
v = find(b);
```

If they belong to different components,

merge them.

```java
parent[u] = v;
```

Otherwise,

the cable is **redundant** because it forms a cycle.

---

### Step 4

Count how many root nodes remain.

Each root represents one connected component.

---

### Step 5

If there are

```text
components
```

connected components,

minimum operations required are

```text
components - 1
```

---

# 🧪 Dry Run

### Input

```text
n = 4

connections =
[
 [0,1],
 [0,2],
 [1,2]
]
```

### Initial Parent

```text
0 1 2 3
```

---

### Process (0,1)

Different parents.

Union.

```text
1 1 2 3
```

---

### Process (0,2)

Root of 0 = 1

Root of 2 = 2

Union.

```text
1 2 2 3
```

---

### Process (1,2)

Root of 1 = 2

Root of 2 = 2

Already connected.

This edge forms a cycle.

No union.

---

### Parent After Path Compression

```text
2 2 2 3
```

Components

```text
{0,1,2}

{3}
```

Number of components

```text
2
```

Answer

```text
components - 1

2 - 1 = 1
```

---

# ✅ Java Solution

```java
class Solution {

    int[] parent;

    int find(int x){
        if(parent[x] == x)
            return x;

        return parent[x] = find(parent[x]);
    }

    public int makeConnected(int n, int[][] connections) {

        if(connections.length < n - 1)
            return -1;

        parent = new int[n];

        for(int i = 0; i < n; i++)
            parent[i] = i;

        for(int[] e : connections){

            int u = find(e[0]);
            int v = find(e[1]);

            if(u != v)
                parent[u] = v;
        }

        int components = 0;

        for(int i = 0; i < n; i++)
            if(find(i) == i)
                components++;

        return components - 1;
    }
}
```

---

# ✅ Correctness

The algorithm correctly identifies connected components using Union-Find.

* If two nodes belong to different components, they are merged.
* If they already belong to the same component, the edge is redundant.
* After processing all edges, every root node represents one connected component.
* Connecting `k` components always requires exactly `k - 1` cables.

Therefore, the algorithm returns the minimum number of operations required.

---

# ⏱ Complexity Analysis

### Time Complexity

Each `find()` operation uses **path compression**, making it almost constant time.

Overall complexity:

```text
O(E × α(N))
```

where

* `E` = number of connections
* `α(N)` = Inverse Ackermann Function (practically constant)

---

### Space Complexity

```text
O(N)
```

for the parent array.

---

# 🧠 Pattern Recognition

Use **Disjoint Set Union (Union-Find)** whenever you see:

* Connected components
* Merge groups
* Detect cycles
* Dynamic connectivity
* Network connection problems

---

# 📚 Similar Problems

* LeetCode 547 – Number of Provinces
* LeetCode 684 – Redundant Connection
* LeetCode 721 – Accounts Merge
* LeetCode 947 – Most Stones Removed with Same Row or Column
* LeetCode 990 – Satisfiability of Equality Equations

---

# 🔑 Key Takeaways

* A connected graph with `n` nodes requires at least `n - 1` edges.
* Union-Find efficiently merges connected components.
* An edge connecting two nodes already in the same component is **redundant**.
* If there are `k` connected components, the minimum operations required to connect them is:

```text
k - 1
```

* Union-Find with path compression provides an almost constant-time solution.
