# Valid Binary Search Tree

## Problem Statement

Given the root of a binary tree, determine whether it is a **valid Binary Search Tree (BST)**.

A Binary Search Tree follows these rules:

* All values in the **left subtree** are smaller than the current node.
* All values in the **right subtree** are greater than the current node.
* The same rule must be satisfied recursively for every node.
* Duplicate values are not allowed.

---

## Approach

We use **Recursion + Range Checking**.

For every node, we maintain two boundaries:

```text
min < root.val < max
```

For the:

* **Left subtree**, the maximum allowed value becomes `root.val`.
* **Right subtree**, the minimum allowed value becomes `root.val`.

### Recursive Rules

```text
Left:
check(root.left, min, root.val)

Right:
check(root.right, root.val, max)
```

If a node violates its allowed range:

```text
root.val <= min
OR
root.val >= max
```

then the tree is not a valid BST.

---

## Code

```java
class Solution {

    public boolean check(TreeNode root, long min, long max) {

        // Empty subtree is valid
        if (root == null) {
            return true;
        }

        // Current node must be inside the allowed range
        if (root.val <= min || root.val >= max) {
            return false;
        }

        // Check left subtree
        if (!check(root.left, min, root.val)) {
            return false;
        }

        // Check right subtree
        if (!check(root.right, root.val, max)) {
            return false;
        }

        return true;
    }

    public boolean isValidBST(TreeNode root) {
        return check(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
}
```

---

## Example 1

### Input

```text
        5
       / \
      3   7
     / \
    2   4
```

### Range Checking

For node `5`:

```text
(-∞, +∞)
```

For node `3`:

```text
(-∞, 5)
```

For node `7`:

```text
(5, +∞)
```

For node `2`:

```text
(-∞, 3)
```

For node `4`:

```text
(3, 5)
```

Every node satisfies its range.

### Output

```text
true
```

---

## Example 2

### Input

```text
        5
       / \
      3   7
         /
        4
```

At node `4`, its allowed range is:

```text
(5, 7)
```

because `4` is:

* in the right subtree of `5` → must be greater than `5`
* in the left subtree of `7` → must be smaller than `7`

But:

```text
4 < 5
```

So the condition fails.

### Output

```text
false
```

---

## Why Do We Use `long`?

We use:

```java
long min
long max
```

instead of `int` because the tree can contain values such as:

```text
Integer.MIN_VALUE
Integer.MAX_VALUE
```

Using `Long.MIN_VALUE` and `Long.MAX_VALUE` gives us safe initial boundaries.

---

## Complexity

Let `n` be the number of nodes.

### Time Complexity

```text
O(n)
```

Every node is visited once.

### Space Complexity

```text
O(h)
```

where `h` is the height of the tree.

The space is used by the recursion stack.

For a balanced tree:

```text
O(log n)
```

For a skewed tree:

```text
O(n)
```

---

## Key Pattern

The important pattern for **Valid BST** is:

```text
             root
            /    \
           /      \
      (min,root) (root,max)
```

Remember:

```text
Left  → (min, root.val)
Right → (root.val, max)
```

### Main Idea

> **Every node must lie within the valid range created by all its ancestors.**

This is why checking only:

```text
left < root < right
```

is not sufficient.
