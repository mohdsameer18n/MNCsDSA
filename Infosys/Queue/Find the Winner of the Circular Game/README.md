# Find the Winner of the Circular Game

## Problem

There are `n` friends standing in a circle and numbered from `1` to `n`.

Starting from friend `1`, count the next `k` friends in the clockwise direction.

- The `k`th friend is eliminated from the circle.
- The counting starts again from the friend immediately clockwise of the eliminated friend.
- Repeat this process until only one friend remains.

Return the winner of the game.

---

# Approach: Queue Simulation (ArrayDeque)

## Intuition

The game follows a circular pattern.

A queue (`ArrayDeque`) can efficiently simulate this behavior:

- Store all players in the queue.
- Rotate the first `k - 1` players to the back of the queue.
- Remove the `k`th player.
- Repeat until only one player remains.

The last remaining player is the winner.

---

## Algorithm

1. Create an `ArrayDeque`.
2. Insert players `1` to `n` into the queue.
3. While more than one player remains:
   - Move the first `k - 1` players to the back of the queue.
   - Remove the player at the front (the `k`th player).
4. Return the remaining player.

---

## Dry Run

### Example

```text
n = 5
k = 2
```

Initial Queue:

```text
[1, 2, 3, 4, 5]
```

### Round 1

Move first player to the back:

```text
[2, 3, 4, 5, 1]
```

Remove front:

```text
2 eliminated

Queue:
[3, 4, 5, 1]
```

---

### Round 2

Rotate:

```text
[4, 5, 1, 3]
```

Remove:

```text
4 eliminated

Queue:
[5, 1, 3]
```

---

### Round 3

Rotate:

```text
[1, 3, 5]
```

Remove:

```text
1 eliminated

Queue:
[3, 5]
```

---

### Round 4

Rotate:

```text
[5, 3]
```

Remove:

```text
5 eliminated

Queue:
[3]
```

Winner:

```text
3
```

---

## Why Does This Work?

The queue always maintains the current circular order of players.

For each round:

- Rotating the first `k - 1` players simulates counting around the circle.
- Removing the front player eliminates the `k`th player.
- The player immediately after the eliminated one automatically becomes the new starting point because they are now at the front of the queue.

Thus, the queue accurately simulates the circular game.

---

## Complexity Analysis

Let:

- `n` = number of players
- `k` = counting step

### Time Complexity

- Each elimination performs `k - 1` rotations.
- There are `n - 1` eliminations.

Overall:

```text
O(n × k)
```

### Space Complexity

```text
O(n)
```

The queue stores all players.

---

## Java Solution

```java
import java.util.*;

class Solution {
    public int findTheWinner(int n, int k) {

        ArrayDeque<Integer> deque = new ArrayDeque<>();

        for (int i = 1; i <= n; i++) {
            deque.addLast(i);
        }

        while (deque.size() > 1) {

            for (int count = 1; count < k; count++) {
                deque.addLast(deque.removeFirst());
            }

            deque.removeFirst();
        }

        return deque.peekFirst();
    }
}
```

---

## Key Takeaways

- A queue (`ArrayDeque`) naturally models circular traversal.
- Rotating the first `k - 1` players simulates counting in a circle.
- Removing the front player eliminates the `k`th player.
- The next player automatically becomes the starting point for the next round.
- This simulation runs in **O(n × k)** time and **O(n)** space.
