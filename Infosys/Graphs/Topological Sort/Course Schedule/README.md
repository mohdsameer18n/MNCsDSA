# Course Schedule

## Problem

There are `numCourses` courses labeled from `0` to `numCourses - 1`.

You are given:

```text
prerequisites[i] = [course, prerequisite]
```

This means:

```text
To take `course`,
you must first complete `prerequisite`.
```

Return `true` if it is possible to finish all courses. Otherwise, return `false`.

**LeetCode:** [207. Course Schedule](https://leetcode.com/problems/course-schedule/)

---

## Example

### Example 1

```text
numCourses = 2
prerequisites = [[1,0]]
```

Graph:

```text
0 → 1
```

We can complete:

```text
Course 0
   ↓
Course 1
```

Output:

```text
true
```

---

### Example 2

```text
numCourses = 2
prerequisites = [[1,0],[0,1]]
```

Graph:

```text
0 → 1
↑   ↓
└───┘
```

There is a cycle:

```text
0 → 1 → 0
```

Therefore, it is impossible to finish all courses.

Output:

```text
false
```

---

# Approach — BFS / Kahn's Algorithm

This problem can be solved using:

```text
Directed Graph
      ↓
Indegree
      ↓
Queue
      ↓
Topological Sort
      ↓
Cycle Detection
```

---

## What is Indegree?

`indegree[i]` represents the number of prerequisites that course `i` currently has.

For:

```text
0 → 1 → 2
```

we have:

```text
indegree[0] = 0
indegree[1] = 1
indegree[2] = 1
```

A course with:

```text
indegree = 0
```

has no remaining prerequisites, so we can take it immediately.

---

## Step 1 — Build the Graph

For:

```text
[1,0]
```

course `1` requires course `0`.

So we create:

```text
0 → 1
```

Code:

```java
graph.get(prerequisite).add(course);
indegree[course]++;
```

---

## Step 2 — Add Courses With Indegree 0

```java
for (int i = 0; i < numCourses; i++) {

    if (indegree[i] == 0) {
        q.offer(i);
    }
}
```

These courses can be completed immediately.

---

## Step 3 — Process the Queue

Take a course:

```java
int course = q.poll();
count++;
```

After completing it, we remove it as a prerequisite from all its neighboring courses.

For:

```text
course → next
```

we do:

```java
indegree[next]--;
```

### Important

Do **not** decrease:

```java
indegree[course]--;
```

We decrease:

```java
indegree[next]--;
```

because `course` was one of the prerequisites of `next`.

If:

```java
indegree[next] == 0
```

then all prerequisites of `next` are complete:

```java
q.offer(next);
```

---

# Java Code

```java
class Solution {

    public boolean canFinish(int numCourses, int[][] prerequisites) {

        List<List<Integer>> graph = new ArrayList<>();

        // Create graph
        for (int i = 0; i < numCourses; i++) {
            graph.add(new ArrayList<>());
        }

        int[] indegree = new int[numCourses];

        // Build graph and calculate indegree
        for (int[] p : prerequisites) {

            int course = p[0];
            int prerequisite = p[1];

            graph.get(prerequisite).add(course);
            indegree[course]++;
        }

        Queue<Integer> q = new ArrayDeque<>();

        // Add courses with no prerequisites
        for (int i = 0; i < numCourses; i++) {

            if (indegree[i] == 0) {
                q.offer(i);
            }
        }

        int count = 0;

        // Topological Sort
        while (!q.isEmpty()) {

            int course = q.poll();
            count++;

            for (int next : graph.get(course)) {

                // Remove current course as a prerequisite
                indegree[next]--;

                if (indegree[next] == 0) {
                    q.offer(next);
                }
            }
        }

        // If every course was processed, no cycle exists
        return count == numCourses;
    }
}
```

---

# Dry Run

Consider:

```text
numCourses = 4

prerequisites = [
    [1,0],
    [2,1],
    [3,2]
]
```

Graph:

```text
0 → 1 → 2 → 3
```

Initial:

```text
indegree = [0,1,1,1]
queue = [0]
count = 0
```

---

### Process Course 0

```text
course = 0
count = 1
```

Neighbor:

```text
next = 1
```

Decrease:

```text
indegree[1]--
```

Now:

```text
indegree = [0,0,1,1]
queue = [1]
```

---

### Process Course 1

```text
course = 1
count = 2
```

Neighbor:

```text
next = 2
```

Decrease:

```text
indegree[2]--
```

Now:

```text
indegree = [0,0,0,1]
queue = [2]
```

---

### Process Course 2

```text
course = 2
count = 3
```

Neighbor:

```text
next = 3
```

Decrease:

```text
indegree[3]--
```

Now:

```text
indegree = [0,0,0,0]
queue = [3]
```

---

### Process Course 3

```text
course = 3
count = 4
```

No neighbors.

Finally:

```text
count = 4
numCourses = 4
```

Therefore:

```java
return count == numCourses;
```

returns:

```text
true
```

---

# Cycle Detection

Consider:

```text
0 → 1
↑   ↓
└───┘
```

Prerequisites:

```text
[[1,0],[0,1]]
```

Initial:

```text
indegree = [1,1]
```

There is no course with:

```text
indegree = 0
```

Therefore:

```text
queue = []
```

Nothing can be processed.

So:

```text
count = 0
numCourses = 2
```

Finally:

```java
return count == numCourses;
```

becomes:

```text
0 == 2
false
```

The cycle prevents us from completing all courses.

---

# Why Does `count == numCourses` Detect a Cycle?

In a graph without a cycle:

```text
Every course
    ↓
Can eventually be processed
    ↓
count == numCourses
```

In a graph with a cycle:

```text
Cycle exists
    ↓
Courses inside cycle never reach indegree 0
    ↓
They are never added to queue
    ↓
count < numCourses
```

Therefore:

```java
count == numCourses
```

means all courses can be completed.

---

# Important Pattern

```text
Course Schedule
       ↓
Directed Graph
       ↓
Calculate Indegree
       ↓
Add Indegree 0 to Queue
       ↓
BFS
       ↓
Decrease Neighbor Indegree
       ↓
Neighbor Indegree = 0
       ↓
Add to Queue
       ↓
count == numCourses
```

### Remember

For an edge:

```text
A → B
```

when `A` is completed:

```java
indegree[B]--;
```

not:

```java
indegree[A]--;
```

---

## Complexity

Let:

* `V` = number of courses
* `E` = number of prerequisite relationships

```text
Time  : O(V + E)
Space : O(V + E)
```

---

## Pattern Classification

```text
Graph
 ↓
Directed Graph
 ↓
Topological Sort
 ↓
BFS
 ↓
Kahn's Algorithm
 ↓
Indegree
 ↓
Cycle Detection
```

**Pattern:** `Directed Graph + BFS + Topological Sort + Indegree`
