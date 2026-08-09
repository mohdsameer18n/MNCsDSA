# Binary Tree Maximum Path Sum

## Problem Statement

Given the root of a binary tree, find the **maximum path sum**.

A path can start and end at **any node** in the tree, but it must follow parent-child connections and cannot visit the same node more than once.

---

## Example

### Input

```text
        -10
        /  \
       9    20
           /  \
          15   7
```

### Output

```text
42
```

### Explanation

The maximum path is:

```text
15 → 20 → 7
```

Sum:

```text
15 + 20 + 7 = 42
```

---

# Approach

We use **DFS (Depth-First Search)** with recursion.

For every node, we calculate two things:

### 1. Maximum path that can be returned to the parent

The parent can continue through **only one child**.

Therefore:

```text
root.val + max(left, right)
```

### 2. Maximum path passing through the current node

A path can use both the left and right subtrees:

```text
left + root.val + right
```

We use this value to update the global maximum.

---

## Important Difference

This is the most important concept in the problem.

For:

```text
       20
      /  \
     15   7
```

The path through `20` is:

```text
15 + 20 + 7 = 42
```

So:

```text
maxSum = 42
```

But we **cannot return 42 to the parent**.

Why?

Because if the parent connects to `20`, it would create a path using:

```text
15 → 20 → 7
          ↑
       parent
```

That would branch in two directions.

Therefore, we return only the better branch:

```text
20 + max(15, 7)
= 35
```

### Remember

```text
GLOBAL ANSWER:
left + root + right

RETURN TO PARENT:
root + max(left, right)
```

---

# Handling Negative Values

Suppose:

```text
       5
      / \
    -10  3
```

The `-10` subtree decreases the path sum, so we should ignore it.

We use:

```java
Math.max(0, dfs(root.left))
```

and:

```java
Math.max(0, dfs(root.right))
```

This means:

```text
negative contribution → 0
positive contribution → keep it
```

---

# Java Code

```java
class Solution {

    int maxSum = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        dfs(root);
        return maxSum;
    }

    public int dfs(TreeNode root) {

        // Base case
        if (root == null) {
            return 0;
        }

        // Get left contribution
        // Ignore it if it is negative
        int left = Math.max(0, dfs(root.left));

        // Get right contribution
        // Ignore it if it is negative
        int right = Math.max(0, dfs(root.right));

        // Path passing through current node
        int currentPath = left + root.val + right;

        // Update global maximum
        maxSum = Math.max(maxSum, currentPath);

        // Return only one branch to parent
        return root.val + Math.max(left, right);
    }
}
```

---

# Dry Run

Consider:

```text
        -10
        /  \
       9    20
           /  \
          15   7
```

## Node 9

```text
left = 0
right = 0

currentPath = 0 + 9 + 0
            = 9

maxSum = 9

return 9
```

---

## Node 15

```text
currentPath = 15

maxSum = 15

return 15
```

---

## Node 7

```text
currentPath = 7

maxSum = 15

return 7
```

---

## Node 20

Left contribution:

```text
15
```

Right contribution:

```text
7
```

Path through `20`:

```text
15 + 20 + 7
= 42
```

Update:

```text
maxSum = 42
```

Return to parent:

```text
20 + max(15, 7)
= 35
```

Notice:

```text
42 → global answer
35 → returned to parent
```

---

## Node -10

Left contribution:

```text
9
```

Right contribution:

```text
35
```

Path through `-10`:

```text
9 + (-10) + 35
= 34
```

Since:

```text
maxSum = max(42, 34)
       = 42
```

Final answer:

```text
42
```

---

# Algorithm

```text
1. Start DFS from root.
2. Recursively calculate the left contribution.
3. Recursively calculate the right contribution.
4. Ignore negative contributions using max(0, contribution).
5. Calculate:
       left + root + right
6. Update the global maximum.
7. Return:
       root + max(left, right)
```

---

# Why Postorder DFS?

We need the results from:

```text
left subtree
right subtree
```

before calculating the result for the current node.

Therefore, the traversal is:

```text
Left → Right → Root
```

This is **Postorder DFS**.

---

# Complexity

Let `n` be the number of nodes and `h` be the height of the tree.

### Time Complexity

```text
O(n)
```

Every node is visited exactly once.

### Space Complexity

```text
O(h)
```

because of the recursion stack.

For a balanced tree:

```text
O(log n)
```

For a completely skewed tree:

```text
O(n)
```

---

# Important Pattern

This problem follows the pattern:

```text
Binary Tree
    ↓
DFS / Recursion
    ↓
Postorder
    ↓
Get left contribution
    ↓
Get right contribution
    ↓
Calculate answer through current node
    ↓
Return only one branch
```

## Memory Trick

```text
             ROOT
           /      \
        LEFT      RIGHT

Global Answer:
LEFT + ROOT + RIGHT

Return:
ROOT + max(LEFT, RIGHT)
```

### One-Line Rule

> **Use both sides for the global answer, but return only one side to the parent.**

---

## Pattern Classification

```text
Pattern     : Binary Tree DFS
Technique   : Recursion
Traversal   : Postorder
Concept     : Global Maximum + Return Value
Time        : O(n)
Space       : O(h)
```
