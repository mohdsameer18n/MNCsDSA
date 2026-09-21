# Linked List Cycle

## Problem

Given the head of a singly linked list, determine whether the linked list contains a cycle.

A cycle exists if a node can be reached again by continuously following the `next` pointer.

---

## Approach

We use **Floyd's Cycle Detection Algorithm**, also known as the **Tortoise and Hare Algorithm**.

We maintain two pointers:

* `slow` moves one node at a time.
* `fast` moves two nodes at a time.

If a cycle exists, `slow` and `fast` will eventually meet.

If `fast` reaches `null`, there is no cycle.

---

## Algorithm

1. Initialize `slow` and `fast` to `head`.
2. Move `slow` one step.
3. Move `fast` two steps.
4. If `slow == fast`, a cycle exists.
5. If `fast == null` or `fast.next == null`, there is no cycle.
6. Return the result.

---

## Java Code

```java
public class Main {

    static class ListNode {
        int val;
        ListNode next;

        ListNode(int x) {
            val = x;
            next = null;
        }
    }

    public static boolean hasCycle(ListNode head) {

        ListNode fast = head;
        ListNode slow = head;

        while (fast != null && fast.next != null) {

            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) {
                return true;
            }
        }

        return false;
    }

    public static void main(String[] args) {

        // Create nodes
        ListNode head = new ListNode(1);
        ListNode second = new ListNode(2);
        ListNode third = new ListNode(3);
        ListNode fourth = new ListNode(4);

        // Connect nodes
        head.next = second;
        second.next = third;
        third.next = fourth;

        // Create cycle: 4 -> 2
        fourth.next = second;

        if (hasCycle(head)) {
            System.out.println("Cycle exists");
        } else {
            System.out.println("No cycle");
        }
    }
}
```

## Example

The linked list created in the above code is:

```text
1 → 2 → 3 → 4
    ↑       ↓
    └───────┘
```

### Output

```text
Cycle exists
```

---

## Complexity

| Complexity | Value  |
| ---------- | ------ |
| Time       | `O(n)` |
| Space      | `O(1)` |

The algorithm uses only two pointers, so it requires constant extra space.

---

## Key Concept

The important condition is:

```java
while (fast != null && fast.next != null)
```

This prevents accessing `fast.next.next` when `fast` or `fast.next` is `null`.

### Floyd's Algorithm

```text
slow → 1 step
fast → 2 steps

If cycle:
slow == fast → Cycle exists

If no cycle:
fast reaches null → No cycle
```
