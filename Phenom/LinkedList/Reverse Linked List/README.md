# Reverse Linked List

## Problem

Given the head of a singly linked list, reverse the linked list and return the new head.

### Example

**Input:**

```text
1 -> 2 -> 3 -> 4 -> 5 -> null
```

**Output:**

```text
5 -> 4 -> 3 -> 2 -> 1 -> null
```

---

## Approach

We use an **iterative approach** with three pointers:

* `curr` – points to the current node.
* `prev` – points to the previous node.
* `next` – temporarily stores the next node so we don't lose the remaining list.

### Steps

For every node:

1. Store the next node:

   ```java
   next = curr.next;
   ```

2. Reverse the current node's pointer:

   ```java
   curr.next = prev;
   ```

3. Move `prev` forward:

   ```java
   prev = curr;
   ```

4. Move `curr` forward:

   ```java
   curr = next;
   ```

When `curr` becomes `null`, `prev` points to the new head.

---

## Code

```java
class Solution {
    public ListNode reverseList(ListNode head) {

        ListNode curr = head;
        ListNode prev = null;
        ListNode next;

        while (curr != null) {
            // Store next node
            next = curr.next;

            // Reverse the link
            curr.next = prev;

            // Move prev forward
            prev = curr;

            // Move curr forward
            curr = next;
        }

        return prev;
    }
}
```

---

## Dry Run

For:

```text
1 -> 2 -> 3 -> null
```

### Initial

```text
prev = null
curr = 1
```

### Iteration 1

```text
next = 2
1.next = null
prev = 1
curr = 2
```

List:

```text
1 -> null

2 -> 3 -> null
```

### Iteration 2

```text
next = 3
2.next = 1
prev = 2
curr = 3
```

List:

```text
2 -> 1 -> null

3 -> null
```

### Iteration 3

```text
next = null
3.next = 2
prev = 3
curr = null
```

Final:

```text
3 -> 2 -> 1 -> null
```

Return:

```text
prev
```

---

## Complexity

| Complexity | Value    |
| ---------- | -------- |
| Time       | **O(n)** |
| Space      | **O(1)** |

The list is traversed once, and only a constant number of pointers are used.

## Key Concept

The important operation is:

```java
curr.next = prev;
```

This changes the direction of the linked-list pointer one node at a time.
