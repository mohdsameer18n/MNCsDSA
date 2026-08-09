# House Robber III

## Problem

You are given the root of a binary tree. Each node represents a house containing some amount of money.

You cannot rob two directly connected houses.

Return the maximum amount of money you can rob without alerting the police.

**LeetCode:** [337. House Robber III](https://leetcode.com/problems/house-robber-iii/)

---

## Approach — Tree DP

For every tree node, we maintain two DP states:

```text
dp[0] = Maximum money when the current node is robbed
dp[1] = Maximum money when the current node is NOT robbed
```

### Case 1: Rob the current node

If we rob the current node, we cannot rob its children.

```text
rob = root.val + left[1] + right[1]
```

### Case 2: Don't rob the current node

If we don't rob the current node, each child can either be robbed or not robbed.

```text
notRob =
    max(left[0], left[1])
    + max(right[0], right[1])
```

For a `null` node:

```text
{0, 0}
```

---

## Java Code

```java
class Solution {

    public int[] solve(TreeNode root) {

        if (root == null) {
            return new int[]{0, 0};
        }

        int[] left = solve(root.left);
        int[] right = solve(root.right);

        // Rob current node
        int rob = root.val + left[1] + right[1];

        // Don't rob current node
        int notRob = Math.max(left[0], left[1])
                   + Math.max(right[0], right[1]);

        return new int[]{rob, notRob};
    }

    public int rob(TreeNode root) {

        int[] res = solve(root);

        return Math.max(res[0], res[1]);
    }
}
```

---

## Example

### Input

```text
        3
       / \
      2   3
       \   \
        3   1
```

### DP Calculation

For node `2`:

```text
rob = 2
notRob = 3
```

For node `3` on the right:

```text
rob = 3
notRob = 1
```

For the root `3`:

```text
rob = 3 + 3 + 1
    = 7

notRob = max(2, 3) + max(3, 1)
       = 3 + 3
       = 6
```

Therefore:

```text
Answer = max(7, 6)
       = 7
```

### Output

```text
7
```

---

## Complexity

Let `n` be the number of nodes.

* **Time:** `O(n)` — every node is processed once.
* **Space:** `O(h)` — recursion stack, where `h` is the height of the tree.

---

## DP Pattern

This problem follows the **Tree DP with 2 states** pattern:

```text
              Node
             /    \
           Left   Right

        ┌─────────────────┐
        │  ROB / NOT ROB  │
        └─────────────────┘
```

The important rule is:

```text
If ROB current
    → children must NOT be robbed

If NOT ROB current
    → children can be ROB or NOT ROB
```

### Key Formula

```text
ROB:
root.val + left.NOT_ROB + right.NOT_ROB

NOT_ROB:
max(left.ROB, left.NOT_ROB)
+
max(right.ROB, right.NOT_ROB)
```

**Pattern:** `Tree + DP + 2 States`
