# Linked List Cycle II

## Problem

Given the head of a linked list, return the node where the cycle begins.

If there is no cycle, return `null`.

A cycle exists when a node's `next` pointer points back to a previous node in the linked list.

---

## Understanding `pos`

In the LeetCode problem, `pos` indicates the index of the node that the tail connects to.

The index is **0-based**.

For example:

```text
Input: head = [3,2,0,-4], pos = 1
```

The linked list looks like:

```text
3 → 2 → 0 → -4
    ↑         |
    └─────────┘
```

The tail `-4` connects to the node at index `1`, which contains `2`.

Therefore, the cycle starts at node `2`.

> Note: `pos` is used by LeetCode to create the linked list. It is **not passed as a parameter** to your `detectCycle()` method.

---

## Examples

### Example 1

```text
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
```

The cycle starts at:

```text
2
```

---

### Example 2

```text
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
```

The linked list is:

```text
1 → 2
↑   |
└───┘
```

The cycle starts at:

```text
1
```

---

### Example 3

```text
Input: head = [1], pos = -1
Output: no cycle
```

There is no cycle:

```text
1 → null
```

The method returns:

```java
null
```

---

## Approach

We use **Floyd's Cycle Detection Algorithm**, also called the **Tortoise and Hare Algorithm**.

We use two pointers:

* `slow` moves one step at a time.
* `fast` moves two steps at a time.

### Step 1: Detect the Cycle

If a cycle exists, `slow` and `fast` will eventually meet.

```java
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;

    if (slow == fast) {
        // Cycle detected
    }
}
```

### Step 2: Find the Beginning of the Cycle

After `slow` and `fast` meet:

1. Create another pointer `temp`.
2. Set `temp = head`.
3. Move `temp` and `slow` one step at a time.
4. The node where they meet is the beginning of the cycle.

```java
ListNode temp = head;

while (temp != slow) {
    temp = temp.next;
    slow = slow.next;
}

return temp;
```

---

## Java Solution

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */

public class Solution {

    public ListNode detectCycle(ListNode head) {

        ListNode slow = head;
        ListNode fast = head;

        // Step 1: Detect cycle
        while (fast != null && fast.next != null) {

            slow = slow.next;
            fast = fast.next.next;

            // Cycle detected
            if (slow == fast) {

                // Step 2: Find cycle starting point
                ListNode temp = head;

                while (temp != slow) {
                    temp = temp.next;
                    slow = slow.next;
                }

                return temp;
            }
        }

        // No cycle
        return null;
    }
}
```

---

## Why Does the Second Step Work?

Suppose the linked list is:

```text
3 → 2 → 0 → -4
    ↑         |
    └─────────┘
```

The cycle begins at `2`.

After `slow` and `fast` meet inside the cycle:

```text
temp = head
slow = meeting point
```

Move both one step at a time:

```text
temp → ...
slow → ...
```

They will meet exactly at:

```text
2
```

Therefore, `2` is the starting node of the cycle.

---

## Complexity

| Complexity | Value  |
| ---------- | ------ |
| Time       | `O(n)` |
| Space      | `O(1)` |

### Why `O(1)` Space?

We only use three pointers:

```text
slow
fast
temp
```

No extra array, HashSet, or other data structure is required.

---

## Key Difference

### Linked List Cycle

Returns whether a cycle exists:

```java
true
```

or

```java
false
```

### Linked List Cycle II

Returns the actual node where the cycle begins:

```java
ListNode
```

or:

```java
null
```

---

## Key Code to Remember

```java
ListNode slow = head;
ListNode fast = head;

while (fast != null && fast.next != null) {

    slow = slow.next;
    fast = fast.next.next;

    if (slow == fast) {

        ListNode temp = head;

        while (temp != slow) {
            temp = temp.next;
            slow = slow.next;
        }

        return temp;
    }
}

return null;
```

**Algorithm:** Floyd's Cycle Detection
**Time:** `O(n)`
**Space:** `O(1)`
