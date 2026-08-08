# Construct Binary Tree from Preorder and Inorder Traversal

## Problem Statement

Given two integer arrays:

* `preorder` — the preorder traversal of a binary tree.
* `inorder` — the inorder traversal of the same binary tree.

Construct and return the original binary tree.

### Traversals

**Preorder:**

```text
Root → Left → Right
```

**Inorder:**

```text
Left → Root → Right
```

---

## Example

### Input

```text
preorder = [3, 9, 20, 15, 7]
inorder  = [9, 3, 15, 20, 7]
```

### Output

```text
        3
       / \
      9   20
         /  \
        15   7
```

---

## Approach

The key observation is:

```text
Preorder → First element is always the ROOT
Inorder  → Elements before root are LEFT subtree
            Elements after root are RIGHT subtree
```

### Step 1: Use Preorder to find the root

For:

```text
preorder = [3, 9, 20, 15, 7]
```

The first element is:

```text
3
```

So `3` is the root.

### Step 2: Find the root in Inorder

```text
inorder = [9, 3, 15, 20, 7]
             ↑
             3
```

Elements before `3`:

```text
[9]
```

These belong to the left subtree.

Elements after `3`:

```text
[15, 20, 7]
```

These belong to the right subtree.

### Step 3: Recursively build the subtrees

For the left subtree:

```text
preorder → 9
inorder  → 9
```

For the right subtree:

```text
preorder → 20, 15, 7
inorder  → 15, 20, 7
```

The same process is repeated recursively.

---

## Optimization Using HashMap

A simple solution searches for the root inside the `inorder` array every time.

That search takes:

```text
O(n)
```

for each node, resulting in:

```text
O(n²)
```

### Optimization

Store every inorder value and its index in a `HashMap`.

```text
inorder = [9, 3, 15, 20, 7]

HashMap:

9  → 0
3  → 1
15 → 2
20 → 3
7  → 4
```

Now we can find the root position in:

```text
O(1)
```

instead of searching through the array.

---

## Java Code

```java
import java.util.*;

class Solution {

    int preIndex = 0;
    HashMap<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {

        // Store inorder value -> index
        for (int i = 0; i < inorder.length; i++) {
            map.put(inorder[i], i);
        }

        return solve(preorder, 0, inorder.length - 1);
    }

    public TreeNode solve(int[] preorder, int left, int right) {

        // No elements
        if (left > right) {
            return null;
        }

        // Preorder gives the root
        int value = preorder[preIndex++];

        TreeNode root = new TreeNode(value);

        // Find root position in O(1)
        int index = map.get(value);

        // Build left subtree
        root.left = solve(preorder, left, index - 1);

        // Build right subtree
        root.right = solve(preorder, index + 1, right);

        return root;
    }
}
```

---

## Dry Run

```text
preorder = [3, 9, 20, 15, 7]
inorder  = [9, 3, 15, 20, 7]
```

### Root = 3

```text
Inorder:

[9]  3  [15,20,7]
```

Therefore:

```text
Left  → [9]
Right → [15,20,7]
```

### Left subtree

Next preorder element:

```text
9
```

So:

```text
    3
   /
  9
```

### Right subtree

Next preorder element:

```text
20
```

Inorder:

```text
[15] 20 [7]
```

Therefore:

```text
       3
      / \
     9   20
        /  \
       15   7
```

---

## Complexity

| Operation       | Complexity |
| --------------- | ---------: |
| Build HashMap   |       O(n) |
| Construct Tree  |       O(n) |
| **Total Time**  |   **O(n)** |
| HashMap Space   |       O(n) |
| Recursion Stack |       O(n) |
| **Total Space** |   **O(n)** |

---

## Important Pattern

Remember this for interviews:

```text
PREORDER
   ↓
Find ROOT
   ↓
INORDER
   ↓
LEFT | ROOT | RIGHT
   ↓
Recursive calls
```

### One-line memory trick

> **Preorder tells us WHAT the root is, Inorder tells us WHERE the root splits the tree.**

The `HashMap` makes finding that split position **O(1)**.
