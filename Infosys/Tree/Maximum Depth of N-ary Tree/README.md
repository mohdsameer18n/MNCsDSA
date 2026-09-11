# 559. Maximum Depth of N-ary Tree

## Problem

Given the root of an **N-ary tree**, return its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

An N-ary tree is a tree in which each node can have zero or more children.

---

## Example

```text
        1
      / | \
     3  2  4
    / \
   5   6
```

The longest path is:

```text
1 → 3 → 5
```

Therefore:

```text
Maximum Depth = 3
```

---

## Approach — DFS

Use **Depth First Search (DFS)** with recursion.

For every node:

1. If the node is `null`, return `0`.
2. Visit every child recursively.
3. Find the maximum depth among all children.
4. Add `1` for the current node.

### Formula

```text
depth(node) = 1 + max(depth(child))
```

For a leaf node, there are no children, so:

```text
depth(leaf) = 1
```

---

## Java Code

```java
class Solution {
    public int maxDepth(Node root) {
        if (root == null) {
            return 0;
        }

        int max = 0;

        for (Node child : root.children) {
            max = Math.max(max, maxDepth(child));
        }

        return max + 1;
    }
}
```

---

## Dry Run

For:

```text
        1
      / | \
     3  2  4
    / \
   5   6
```

### Node 5

```text
No children
depth = 1
```

### Node 6

```text
No children
depth = 1
```

### Node 3

```text
max(1, 1) + 1 = 2
```

### Node 2

```text
No children
depth = 1
```

### Node 4

```text
No children
depth = 1
```

### Node 1

```text
max(2, 1, 1) + 1 = 3
```

Answer:

```text
3
```

---

## Complexity

### Time Complexity

```text
O(n)
```

Every node is visited exactly once.

### Space Complexity

```text
O(h)
```

where `h` is the height of the tree, because of the recursion stack.

---

## Key Pattern

This problem is an important **Tree DFS / Recursion** pattern.

Remember:

```text
null → 0

current node → 1 + maximum depth of children
```

This same pattern can be applied to many tree problems involving:

* Maximum depth
* Minimum depth
* Tree height
* Longest path
* Recursive subtree calculations
