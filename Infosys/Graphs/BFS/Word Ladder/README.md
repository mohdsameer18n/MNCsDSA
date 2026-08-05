# 127. Word Ladder

## Problem

You are given:

- `beginWord`
- `endWord`
- `wordList`

You can transform one word into another by changing **exactly one character** at a time.

Rules:

- Every transformed word must exist in `wordList`.
- Return the **length of the shortest transformation sequence** from `beginWord` to `endWord`.
- If no transformation is possible, return `0`.

---

## Approach: Breadth First Search (BFS)

### Why BFS?

The problem asks for the **minimum number of transformations**.

Whenever a problem asks for:

- Minimum steps
- Shortest path
- Fewest moves

and every move has the **same cost**, **BFS** is the correct algorithm.

Think of each word as a node.

Example:

```text
hit
 |
hot
/  \
dot lot
|    |
dog log
 \  /
  cog
```

Each edge represents changing **one character**.

The shortest transformation is simply the **shortest path** in this graph.

---

## Algorithm

1. Store all words in a `HashSet` for `O(1)` lookup.
2. If `endWord` does not exist, return `0`.
3. Start BFS from `beginWord`.
4. For every word:
   - Change each character.
   - Try replacing it with every letter from `'a'` to `'z'`.
   - If the generated word exists in the set:
     - Push it into the queue.
     - Remove it from the set (mark visited).
5. Process the queue level by level.
6. The first time `endWord` is reached, return the current level.

---

## Dry Run

### Input

```text
beginWord = "hit"

endWord = "cog"

wordList =

["hot","dot","dog","lot","log","cog"]
```

Initial state:

```text
Queue = [hit]

Set = {hot,dot,dog,lot,log,cog}

Level = 1
```

### Level 1

Current word:

```text
hit
```

Generate all possible one-letter transformations.

Only valid word:

```text
hot
```

Queue:

```text
hot
```

Level = 2

---

### Level 2

Current:

```text
hot
```

Valid transformations:

```text
dot
lot
```

Queue:

```text
dot
lot
```

Level = 3

---

### Level 3

Process:

```text
dot
```

Generate:

```text
dog
```

Process:

```text
lot
```

Generate:

```text
log
```

Queue:

```text
dog
log
```

Level = 4

---

### Level 4

Current:

```text
dog
```

Generate:

```text
cog
```

Queue:

```text
log
cog
```

Level = 5

---

### Level 5

Current:

```text
cog
```

Destination reached.

Return:

```text
5
```

---

## Why remove from HashSet?

```java
set.remove(newWord);
```

The `HashSet` also acts as the **visited** set.

Without removing visited words, the same word can be inserted into the queue multiple times.

Example:

```text
hot
↑  \
|   \
dot  lot
```

Both `dot` and `lot` could keep adding `hot` repeatedly.

Removing the word immediately ensures every word is processed only once.

---

## Time Complexity

Let:

- `N` = number of words
- `L` = length of each word

For every word:

- We try changing each position.
- At every position we try **26 letters**.

Time Complexity:

```text
O(N × L × 26)

≈ O(N × L)
```

---

## Space Complexity

- HashSet
- Queue

```text
O(N)
```

---

## Java Solution

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {

        HashSet<String> set = new HashSet<>(wordList);

        if (!set.contains(endWord))
            return 0;

        Queue<String> q = new ArrayDeque<>();
        q.offer(beginWord);

        int level = 1;

        while (!q.isEmpty()) {

            int size = q.size();

            for (int i = 0; i < size; i++) {

                String word = q.poll();

                if (word.equals(endWord))
                    return level;

                char[] ch = word.toCharArray();

                for (int j = 0; j < ch.length; j++) {

                    char original = ch[j];

                    for (char c = 'a'; c <= 'z'; c++) {

                        if (c == original)
                            continue;

                        ch[j] = c;

                        String newWord = new String(ch);

                        if (set.contains(newWord)) {

                            q.offer(newWord);
                            set.remove(newWord);
                        }
                    }

                    ch[j] = original;
                }
            }

            level++;
        }

        return 0;
    }
}
```
