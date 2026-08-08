# Kth Smallest Element in a BST

## Problem Statement

Given the root of a **Binary Search Tree (BST)** and an integer `k`, return the **k-th smallest value** in the BST.

---

## Key Observation

A Binary Search Tree follows:

```text
Left < Root < Right
```

Therefore, when we perform **Inorder Traversal**:

```text
Left → Root → Right
```

the values are visited in **sorted ascending order**.

For example:

```text
        5
       / \
      3   7
     / \
    2   4
```

Inorder traversal:

```text
2 → 3 → 4 → 5 → 7
```

So:

```text
1st smallest = 2
2nd smallest = 3
3rd smallest = 4
4th smallest = 5
5th smallest = 7
```

Therefore:

> **The k-th node visited during inorder traversal is the k-th smallest element.**

---

## Approach

### Step 1: Perform Inorder Traversal

Traverse the tree using:

```text
Left → Root → Right
```

Store each visited value in an `ArrayList`.

### Step 2: Find the k-th Element

After traversal, the list contains values in sorted order.

Use a counter starting from `1` and return the element when:

```text
count == k
```

---

## Java Code

```java
import java.util.*;

class Solution {

    List<Integer> res = new ArrayList<>();

    public void inOrder(TreeNode root, List<Integer> res) {

        if (root == null) {
            return;
        }

        // Left
        inOrder(root.left, res);

        // Root
        res.add(root.val);

        // Right
        inOrder(root.right, res);
    }

    public int kthSmallest(TreeNode root, int k) {

        inOrder(root, res);

        int count = 1;

        for (int ans : res) {

            if (count == k) {
                return ans;
            }

            count++;
        }

        return -1;
    }
}
```

---

## Dry Run

Consider:

```text
        5
       / \
      3   7
     / \
    2   4
```

And:

```text
k = 3
```

### Inorder Traversal

```text
5
↓
3
↓
2
```

Add `2`:

```text
res = [2]
```

Return to `3`:

```text
res = [2, 3]
```

Go to `4`:

```text
res = [2, 3, 4]
```

Return to `5`:

```text
res = [2, 3, 4, 5]
```

Go to `7`:

```text
res = [2, 3, 4, 5, 7]
```

Now find the 3rd element:

```text
count = 1 → 2
count = 2 → 3
count = 3 → 4
```

Therefore:

```text
Answer = 4
```

---

## Why `ArrayList`?

We use:

```java
List<Integer> res = new ArrayList<>();
```

because `List` is an **interface**, while `ArrayList` is a class that implements `List`.

Incorrect:

```java
List<Integer> res = new List<>();
```

Correct:

```java
List<Integer> res = new ArrayList<>();
```

---

## Complexity

Let `n` be the number of nodes.

### Time Complexity

```text
O(n)
```

In the worst case, we visit every node.

### Space Complexity

```text
O(n)
```

We store all inorder values in the `ArrayList`.

The recursion stack additionally uses:

```text
O(h)
```

where `h` is the height of the tree.

---

## Important Pattern

For BST problems, remember:

```text
BST
 ↓
Inorder Traversal
 ↓
Sorted Order
```

Therefore:

```text
Kth Smallest in BST
        ↓
   Inorder DFS
        ↓
    k-th element
```

### Key Rule

> **Inorder traversal of a BST gives values in sorted ascending order.**
