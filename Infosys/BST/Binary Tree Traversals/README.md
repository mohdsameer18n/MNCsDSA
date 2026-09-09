# Binary Tree Traversals

This README covers the three basic **Depth First Search (DFS)** traversals of a Binary Tree:

1. Preorder Traversal
2. Inorder Traversal
3. Postorder Traversal

---

## 1. Preorder Traversal

### Order

**Root → Left → Right**

### LeetCode

**144. Binary Tree Preorder Traversal**

### Java Solution

```java
class Solution {

    public void preOrder(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }

        // Root
        res.add(root.val);

        // Left
        preOrder(root.left, res);

        // Right
        preOrder(root.right, res);
    }

    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();

        preOrder(root, res);

        return res;
    }
}
```

### Complexity

* Time: `O(n)`
* Space: `O(n)` in the worst case due to recursion.

---

## 2. Inorder Traversal

### Order

**Left → Root → Right**

### LeetCode

**94. Binary Tree Inorder Traversal**

### Java Solution

```java
class Solution {

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

    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();

        inOrder(root, res);

        return res;
    }
}
```

### Complexity

* Time: `O(n)`
* Space: `O(n)` in the worst case due to recursion.

> **Important:** Inorder traversal of a Binary Search Tree (BST) produces the values in sorted ascending order.

---

## 3. Postorder Traversal

### Order

**Left → Right → Root**

### LeetCode

**145. Binary Tree Postorder Traversal**

### Java Solution

```java
class Solution {

    public void postOrder(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }

        // Left
        postOrder(root.left, res);

        // Right
        postOrder(root.right, res);

        // Root
        res.add(root.val);
    }

    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();

        postOrder(root, res);

        return res;
    }
}
```

### Complexity

* Time: `O(n)`
* Space: `O(n)` in the worst case due to recursion.

---

# Example

Consider the following binary tree:

```text
        1
       / \
      2   3
     / \
    4   5
```

### Preorder

**Root → Left → Right**

```text
1 → 2 → 4 → 5 → 3
```

### Inorder

**Left → Root → Right**

```text
4 → 2 → 5 → 1 → 3
```

### Postorder

**Left → Right → Root**

```text
4 → 5 → 2 → 3 → 1
```

---

# Easy Way to Remember

| Traversal     | Order               | Root Position |
| ------------- | ------------------- | ------------- |
| **Preorder**  | Root → Left → Right | First         |
| **Inorder**   | Left → Root → Right | Middle        |
| **Postorder** | Left → Right → Root | Last          |

### The Main Pattern

All three use the same recursive structure:

```java
if (root == null) {
    return;
}

left subtree;
root;
right subtree;
```

The only difference is **where `res.add(root.val)` is placed**.

```text
Preorder:   add → left → right
Inorder:    left → add → right
Postorder:  left → right → add
```

---

# LeetCode Problems

* **94** — Binary Tree Inorder Traversal
* **144** — Binary Tree Preorder Traversal
* **145** — Binary Tree Postorder Traversal

These three problems are fundamental for learning **Binary Tree DFS and recursion**.
