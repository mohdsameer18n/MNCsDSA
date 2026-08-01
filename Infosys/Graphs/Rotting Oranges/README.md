# Rotting Oranges

## Approach: Multi-Source BFS

### Idea

Instead of starting BFS from one cell, we start from **all rotten oranges (`2`) simultaneously**.

Why?

Because in one minute, **every rotten orange spreads the rot to its adjacent fresh oranges** at the same time.

So, all initially rotten oranges become the **sources** of BFS.

---

## Algorithm

1. Traverse the grid.
   - Add every rotten orange (`2`) into the queue.
   - Count the number of fresh oranges (`1`).

2. If there are no fresh oranges, return `0`.

3. Perform **level-order BFS**.
   - One BFS level = One minute.
   - For every rotten orange in the current level:
     - Visit its four adjacent cells.
     - If an adjacent cell contains a fresh orange:
       - Make it rotten.
       - Decrease fresh orange count.
       - Push it into the queue.

4. If at least one orange became rotten during this level, increment the minute count.

5. After BFS:
   - If no fresh oranges remain, return the minutes.
   - Otherwise, return `-1`.

---

## Dry Run

### Input

```text
2 1 1
1 1 0
0 1 1
```

Fresh oranges = **6**

Queue initially:

```text
[(0,0)]
```

---

### Minute 0

Process:

```text
2 1 1
1 1 0
0 1 1
```

Rot `(0,1)` and `(1,0)`.

Queue becomes:

```text
[(0,1), (1,0)]
```

Fresh = 4

Minutes = 1

---

### Minute 1

Process:

```text
2 2 1
2 1 0
0 1 1
```

Rot:

- `(0,2)`
- `(1,1)`

Queue:

```text
[(0,2), (1,1)]
```

Fresh = 2

Minutes = 2

---

### Minute 2

Process:

```text
2 2 2
2 2 0
0 1 1
```

Rot:

```text
(2,1)
```

Queue:

```text
[(2,1)]
```

Fresh = 1

Minutes = 3

---

### Minute 3

Process:

```text
2 2 2
2 2 0
0 2 1
```

Rot:

```text
(2,2)
```

Queue:

```text
[(2,2)]
```

Fresh = 0

Minutes = 4

---

### Minute 4

No new oranges become rotten.

Queue becomes empty.

Since all fresh oranges are rotten,

Return:

```text
4
```

---

## Why use Multi-Source BFS?

Imagine there are multiple rotten oranges.

```text
2 1 1 1 2
```

All rotten oranges spread **simultaneously**.

A normal BFS from only one rotten orange would incorrectly simulate the spread.

Multi-source BFS correctly models the process by placing **all rotten oranges into the queue initially**.

---

## Time Complexity

- Initial traversal: **O(m × n)**
- BFS traversal: **O(m × n)**

Overall:

```text
O(m × n)
```

---

## Space Complexity

Queue can store at most all cells.

```text
O(m × n)
```

---

## Key Points

- Multi-Source BFS problem.
- Every BFS level represents **one minute**.
- All initially rotten oranges are inserted into the queue first.
- A fresh orange is converted to rotten only once.
- If fresh oranges remain after BFS, return **-1**.
- If there are no fresh oranges initially, return **0**.
