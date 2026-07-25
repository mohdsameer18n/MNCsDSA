# Left View and Right View of a Binary Search Tree (BST)

## Problem Statement

Given a Binary Search Tree (BST), print its:

- **Left View** – The first node visible at each level when viewed from the **left side**.
- **Right View** – The first node visible at each level when viewed from the **right side**.

---

## Example

### Input

```text
7
50 30 70 20 40 60 80
```

### BST

```text
        50
       /  \
     30    70
    / \   / \
  20 40 60 80
```

### Output

**Left View**

```text
50 30 20
```

**Right View**

```text
50 70 80
```

---

# Java Program

```java
import java.util.*;

class Node {
    int data;
    Node left, right;

    Node(int data) {
        this.data = data;
        left = right = null;
    }
}

public class Main {

    static Node root = null;
    static int maxLevel = 0;

    // Insert into BST
    static Node insert(Node root, int data) {

        if (root == null)
            return new Node(data);

        if (data < root.data)
            root.left = insert(root.left, data);
        else if (data > root.data)
            root.right = insert(root.right, data);

        return root;
    }

    // Left View
    static void leftView(Node root, int level) {

        if (root == null)
            return;

        if (level > maxLevel) {
            System.out.print(root.data + " ");
            maxLevel = level;
        }

        leftView(root.left, level + 1);
        leftView(root.right, level + 1);
    }

    // Right View
    static void rightView(Node root, int level) {

        if (root == null)
            return;

        if (level > maxLevel) {
            System.out.print(root.data + " ");
            maxLevel = level;
        }

        rightView(root.right, level + 1);
        rightView(root.left, level + 1);
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();

        for (int i = 0; i < n; i++) {
            root = insert(root, sc.nextInt());
        }

        System.out.print("Left View : ");
        maxLevel = 0;
        leftView(root, 1);

        System.out.println();

        System.out.print("Right View: ");
        maxLevel = 0;
        rightView(root, 1);
    }
}
```

---

# Approach

## Left View

- Traverse the tree using **Root → Left → Right**.
- Maintain a variable `maxLevel`.
- Print the first node encountered at every level.

### Algorithm

1. If the node is `null`, return.
2. If `level > maxLevel`:
   - Print the node.
   - Update `maxLevel`.
3. Traverse the left subtree.
4. Traverse the right subtree.

---

## Right View

- Traverse the tree using **Root → Right → Left**.
- Maintain a variable `maxLevel`.
- Print the first node encountered at every level.

### Algorithm

1. If the node is `null`, return.
2. If `level > maxLevel`:
   - Print the node.
   - Update `maxLevel`.
3. Traverse the right subtree.
4. Traverse the left subtree.

---

# Dry Run

For the BST

```text
        50
       /  \
     30    70
    / \   / \
  20 40 60 80
```

## Left View Traversal

```text
50 → 30 → 20
```

Output

```text
50 30 20
```

---

## Right View Traversal

```text
50 → 70 → 80
```

Output

```text
50 70 80
```

---

# Complexity Analysis

| Operation | Time | Space |
|-----------|------|-------|
| Left View | O(n) | O(h) |
| Right View | O(n) | O(h) |

Where:

- **n** = Number of nodes
- **h** = Height of the tree

---

# Difference Between Left View and Right View

| Left View | Right View |
|-----------|------------|
| Root → Left → Right | Root → Right → Left |
| First node from the left at every level | First node from the right at every level |
| Left subtree visited first | Right subtree visited first |
| Example: 50 30 20 | Example: 50 70 80 |

---

# Key Points

- Both traversals visit every node exactly once.
- `maxLevel` ensures only the first node at each level is printed.
- Reset `maxLevel = 0` before calling each view function.
- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(h)`
