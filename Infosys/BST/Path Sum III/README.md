# Path Sum III

## Problem

Given the root of a binary tree and an integer `targetSum`, return the number of paths where the sum of the node values equals `targetSum`.

A path:

* Must move from parent to child.
* Must move downward.
* Can start and end at any node.
* Does not need to end at a leaf.

**LeetCode:** [437. Path Sum III](https://leetcode.com/problems/path-sum-iii/)

---

## Example

```text
        10
       /  \
      5   -3
     / \    \
    3   2    11
   / \   \
  3  -2   1

targetSum = 8
```

Valid paths:

```text
5 → 3
5 → 2 → 1
-3 → 11
```

Output:

```text
3
```

---

## Approach

We use:

```text
Tree DFS
    ↓
Prefix Sum
    ↓
HashMap
    ↓
Backtracking
```

The HashMap stores the frequency of prefix sums on the **current root-to-node path**.

### Prefix Sum Formula

Suppose:

```text
currentSum = sum from root to current node
```

We need:

```text
currentSum - previousSum = targetSum
```

Therefore:

```text
previousSum = currentSum - targetSum
```

So at every node:

```java
long required = currentSum - target;
```

Then:

```java
count += map.getOrDefault(required, 0);
```

---

## What Does `map.get(required)` Mean?

The map stores:

```text
prefixSum → frequency
```

For example:

```text
map = {
    0  → 1,
    10 → 1,
    15 → 1
}
```

Suppose:

```text
currentSum = 18
target = 8
```

Then:

```text
required = 18 - 8
         = 10
```

Since:

```text
map.get(10) = 1
```

there is one previous prefix sum of `10`.

Therefore:

```text
18 - 10 = 8
```

So one valid path exists.

---

## Why Frequency Is Needed

A prefix sum can appear multiple times.

For example:

```text
prefix sums:

0
5
10
5
13
```

Here `5` appears twice.

If:

```text
currentSum = 13
target = 8
```

then:

```text
required = 13 - 8
         = 5
```

Since:

```text
map.get(5) = 2
```

there are two valid paths.

Therefore, the HashMap must store:

```text
prefix sum → number of occurrences
```

---

## Why `map.put(0L, 1)`?

We initialize:

```java
map.put(0L, 1);
```

This represents the prefix sum before the root.

Example:

```text
    5
   /
  3

target = 8
```

At node `3`:

```text
currentSum = 8

required = 8 - 8
         = 0
```

Because the map contains:

```text
0 → 1
```

the path:

```text
5 → 3
```

is counted.

---

## Backtracking

After processing the left and right subtrees, we remove the current prefix sum:

```java
map.put(
    currentSum,
    map.get(currentSum) - 1
);
```

### Why?

The prefix sum belongs only to the current path.

For example:

```text
        10
       /  \
      5   -3
```

The prefix sums from the `5` branch must not be used while processing the `-3` branch.

So the process is:

```text
Add prefix sum
      ↓
Explore children
      ↓
Remove prefix sum
```

This is **backtracking**.

---

## Java Code

```java
class Solution {

    int target;
    int count = 0;

    Map<Long, Integer> map = new HashMap<>();

    public int pathSum(TreeNode root, int targetSum) {

        target = targetSum;

        // Prefix sum before the root
        map.put(0L, 1);

        dfs(root, 0);

        return count;
    }

    private void dfs(TreeNode root, long currentSum) {

        if (root == null) {
            return;
        }

        // Add current node value
        currentSum += root.val;

        // Find required previous prefix sum
        long required = currentSum - target;

        // Add number of matching prefix sums
        count += map.getOrDefault(required, 0);

        // Store current prefix sum
        map.put(
            currentSum,
            map.getOrDefault(currentSum, 0) + 1
        );

        // Explore left and right subtrees
        dfs(root.left, currentSum);
        dfs(root.right, currentSum);

        // Backtrack
        map.put(
            currentSum,
            map.get(currentSum) - 1
        );
    }
}
```

---

## Dry Run

For:

```text
        10
       /  \
      5   -3
     / \    \
    3   2    11

target = 8
```

Initial:

```text
map = {0=1}
count = 0
```

### Node 10

```text
currentSum = 10
required = 10 - 8 = 2

2 not found
```

Map:

```text
{0=1, 10=1}
```

---

### Node 5

```text
currentSum = 15
required = 15 - 8 = 7

7 not found
```

Map:

```text
{0=1, 10=1, 15=1}
```

---

### Node 3

```text
currentSum = 18
required = 18 - 8 = 10
```

`10` exists.

```text
count = 1
```

Valid path:

```text
5 → 3
```

---

### Node 2

```text
currentSum = 17
required = 17 - 8 = 9
```

Not found.

---

### Node -3

```text
currentSum = 7
required = 7 - 8 = -1
```

Not found.

---

### Node 11

```text
currentSum = 18
required = 18 - 8 = 10
```

`10` exists.

```text
count = 2
```

Valid path:

```text
-3 → 11
```

If the tree also contains:

```text
2 → 1
```

then:

```text
5 → 2 → 1
```

is another valid path, making the answer `3`.

---

## Pattern

```text
Binary Tree
     ↓
DFS
     ↓
Current Prefix Sum
     ↓
currentSum - target
     ↓
HashMap
     ↓
Count Matching Prefix Sums
     ↓
Backtracking
```

### Important Template

```java
currentSum += root.val;

long required = currentSum - target;

count += map.getOrDefault(required, 0);

map.put(currentSum,
        map.getOrDefault(currentSum, 0) + 1);

dfs(root.left, currentSum);
dfs(root.right, currentSum);

map.put(currentSum,
        map.get(currentSum) - 1);
```

---

## Complexity

Let `n` be the number of nodes and `h` be the height of the tree.

```text
Time  : O(n)
Space : O(h)
```

Each node is visited once, and the HashMap stores prefix sums along the current DFS path.

---

## Pattern Classification

```text
Tree
 ↓
DFS
 ↓
Prefix Sum
 ↓
HashMap
 ↓
Backtracking
```

**Pattern:** `Tree DFS + Prefix Sum + HashMap + Backtracking`
