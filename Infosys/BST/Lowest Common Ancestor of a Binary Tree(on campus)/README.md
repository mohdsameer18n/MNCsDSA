# Lowest Common Ancestor (LCA) & Topological Sort

## Lowest Common Ancestor (LCA)

### Definition
The **Lowest Common Ancestor (LCA)** of two nodes `p` and `q` in a tree is the **lowest (deepest) node** that has both `p` and `q` as descendants.

### Example

```text
        3
       / \
      5   1
     / \ / \
    6  2 0  8
      / \
     7   4
```

- LCA(5,1) = 3
- LCA(6,4) = 5
- LCA(7,8) = 3

---

## Approach (Binary Tree)

### Algorithm
1. If `root == null`, return `null`.
2. If `root == p` or `root == q`, return `root`.
3. Recursively search the left subtree.
4. Recursively search the right subtree.
5. If both left and right return non-null, the current node is the LCA.
6. Otherwise, return the non-null child.

### Java Solution

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        if (root == null || root == p || root == q)
            return root;

        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        if (left != null && right != null)
            return root;

        return (left != null) ? left : right;
    }
}
```

### Complexity

- **Time:** `O(N)`
- **Space:** `O(H)` (Recursion stack)

---

## LCA in Binary Search Tree (BST)

### Idea

Use the BST property:

- If both nodes are smaller than root → Move left.
- If both nodes are greater than root → Move right.
- Otherwise, the current node is the LCA.

### Java Solution

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        while (root != null) {

            if (p.val < root.val && q.val < root.val)
                root = root.left;

            else if (p.val > root.val && q.val > root.val)
                root = root.right;

            else
                return root;
        }

        return null;
    }
}
```

### Complexity

- **Time:** `O(H)`
- **Space:** `O(1)`

---

# Topological Sort

## Definition

A **Topological Sort** is a linear ordering of vertices in a **Directed Acyclic Graph (DAG)** such that for every directed edge `u → v`, vertex `u` appears before vertex `v`.

> **Topological Sort is only possible for DAGs.**

### Example

```text
5 → 2 → 3 → 1
↓
0
```

One valid ordering is:

```text
5 4 2 3 1 0
```

---

## Method 1: Kahn's Algorithm (BFS)

### Algorithm

1. Calculate the indegree of every vertex.
2. Insert all vertices with indegree `0` into a queue.
3. Remove a vertex from the queue and add it to the answer.
4. Decrease the indegree of its neighbors.
5. If any neighbor's indegree becomes `0`, push it into the queue.
6. Continue until the queue becomes empty.

### Java Solution

```java
import java.util.*;

class Solution {

    public int[] topoSort(int V, ArrayList<ArrayList<Integer>> adj) {

        int[] indegree = new int[V];

        for (int i = 0; i < V; i++) {
            for (int node : adj.get(i))
                indegree[node]++;
        }

        Queue<Integer> queue = new LinkedList<>();

        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0)
                queue.offer(i);
        }

        int[] ans = new int[V];
        int index = 0;

        while (!queue.isEmpty()) {

            int node = queue.poll();
            ans[index++] = node;

            for (int neighbor : adj.get(node)) {

                indegree[neighbor]--;

                if (indegree[neighbor] == 0)
                    queue.offer(neighbor);
            }
        }

        return ans;
    }
}
```

### Complexity

- **Time:** `O(V + E)`
- **Space:** `O(V)`

---

## Method 2: DFS

### Algorithm

1. Perform DFS from every unvisited node.
2. Visit all neighbors first.
3. Push the current node onto a stack.
4. Pop the stack to obtain the topological ordering.

### Java Solution

```java
import java.util.*;

class Solution {

    void dfs(int node, ArrayList<ArrayList<Integer>> adj,
             boolean[] vis, Stack<Integer> stack) {

        vis[node] = true;

        for (int neighbor : adj.get(node)) {
            if (!vis[neighbor])
                dfs(neighbor, adj, vis, stack);
        }

        stack.push(node);
    }

    public int[] topoSort(int V, ArrayList<ArrayList<Integer>> adj) {

        boolean[] vis = new boolean[V];
        Stack<Integer> stack = new Stack<>();

        for (int i = 0; i < V; i++) {
            if (!vis[i])
                dfs(i, adj, vis, stack);
        }

        int[] ans = new int[V];
        int index = 0;

        while (!stack.isEmpty())
            ans[index++] = stack.pop();

        return ans;
    }
}
```

### Complexity

- **Time:** `O(V + E)`
- **Space:** `O(V)`

---

# BFS vs DFS Topological Sort

| Feature | Kahn's Algorithm (BFS) | DFS |
|----------|------------------------|-----|
| Data Structure | Queue | Stack + Recursion |
| Uses Indegree | ✅ Yes | ❌ No |
| Cycle Detection | Easy | Requires additional recursion stack |
| Time Complexity | `O(V + E)` | `O(V + E)` |
| Space Complexity | `O(V)` | `O(V)` |

---

# Key Interview Points

### Lowest Common Ancestor (LCA)

- Binary Tree LCA uses recursion and divide-and-conquer.
- Time Complexity: **O(N)**
- Space Complexity: **O(H)**
- BST LCA is optimized using BST properties.
- Time Complexity for BST: **O(H)**

### Topological Sort

- Works **only for Directed Acyclic Graphs (DAGs)**.
- **Kahn's Algorithm** uses **Queue + Indegree**.
- **DFS Method** uses **Postorder Traversal + Stack**.
- If Kahn's Algorithm processes **fewer than `V` vertices**, the graph contains a **cycle**, and no valid topological ordering exists.
```
