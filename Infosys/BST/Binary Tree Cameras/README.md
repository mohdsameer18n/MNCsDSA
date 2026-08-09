# Binary Tree Cameras

## Problem

You are given the root of a binary tree. A camera can be installed on any node.

A camera can monitor:

* Its parent
* Itself
* Its immediate children

Return the **minimum number of cameras** needed to monitor every node in the binary tree.

**LeetCode:** [968. Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

---

## Approach

This problem uses **Tree DP / Greedy DFS** with **3 states**.

For every node, we return one of three states:

```text
0 → Node needs a camera
1 → Node is covered
2 → Node has a camera
```

### State 0 — NEED

The current node is not covered and needs a camera.

```text
0 = NEED CAMERA
```

### State 1 — COVERED

The current node is already covered by a camera from one of its children.

```text
1 = COVERED
```

### State 2 — CAMERA

A camera is installed on the current node.

```text
2 = HAS CAMERA
```

---

## Decision Logic

For every node, process its children first using DFS.

### Case 1: A child needs a camera

If either child returns `0`:

```text
left == 0 || right == 0
```

we must put a camera on the current node.

```text
cameras++
return 2
```

---

### Case 2: A child has a camera

If either child returns `2`:

```text
left == 2 || right == 2
```

the current node is already covered.

```text
return 1
```

---

### Case 3: Children are covered

If both children are covered but neither has a camera, the current node is not covered.

Therefore:

```text
return 0
```

The current node now needs a camera.

---

## Java Code

```java
class Solution {

    int cameras = 0;

    public int minCameraCover(TreeNode root) {

        // If root itself needs a camera
        if (dfs(root) == 0) {
            cameras++;
        }

        return cameras;
    }

    // 0 = NEED camera
    // 1 = COVERED
    // 2 = HAS camera

    private int dfs(TreeNode root) {

        // Null nodes are considered covered
        if (root == null) {
            return 1;
        }

        int left = dfs(root.left);
        int right = dfs(root.right);

        // If any child needs a camera,
        // put a camera on the current node
        if (left == 0 || right == 0) {
            cameras++;
            return 2;
        }

        // If any child has a camera,
        // current node is covered
        if (left == 2 || right == 2) {
            return 1;
        }

        // Children are covered,
        // but current node is not covered
        return 0;
    }
}
```

---

## Dry Run

Consider:

```text
        0
       /
      0
     / \
    0   0
```

### Bottom-left node

Its children are `null`.

```text
left  = 1
right = 1
```

Neither child needs a camera and neither has a camera.

Therefore:

```text
return 0
```

The node **needs a camera**.

---

### Parent node

Left child:

```text
0 = NEED
```

Right child:

```text
0 = NEED
```

Since a child needs a camera:

```text
cameras++
return 2
```

So we place a camera:

```text
        0
       /
      📷
     / \
    0   0
```

```text
cameras = 1
```

---

### Root

The left child has a camera:

```text
left = 2
```

Therefore the root is covered:

```text
return 1
```

No additional camera is required.

### Answer

```text
1
```

---

## Important Pattern

Remember the three states:

```text
0 → NEED
1 → COVERED
2 → CAMERA
```

And remember this decision order:

```text
Child needs camera
        ↓
   Put camera here
        ↓
     state = 2

Child has camera
        ↓
     I am covered
        ↓
     state = 1

Otherwise
        ↓
   I need a camera
        ↓
     state = 0
```

### Easy Memory Trick

```text
Child NEED    → I put CAMERA
Child CAMERA  → I am COVERED
Otherwise     → I NEED CAMERA
```

---

## Complexity

Let `n` be the number of nodes and `h` be the height of the tree.

```text
Time  : O(n)
Space : O(h)
```

Each node is visited exactly once, and the recursion stack requires `O(h)` space.

---

## Pattern Classification

```text
Binary Tree
     ↓
DFS
     ↓
Tree DP / Greedy
     ↓
3 States
     ↓
NEED / COVERED / CAMERA
```

**Pattern:** `Tree DP + Greedy DFS + 3 States`
