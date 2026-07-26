# 451. Sort Characters By Frequency

## Problem Statement

Given a string `s`, sort it in **decreasing order** based on the frequency of characters and return the resulting string.

> If multiple characters have the same frequency, their relative order does not matter.

### Example 1

**Input**
```text
s = "tree"
```

**Output**
```text
"eert"
```

**Explanation**

- `e` appears 2 times.
- `t` and `r` appear 1 time each.
- `"eert"` and `"eetr"` are both valid.

---

### Example 2

**Input**
```text
s = "cccaaa"
```

**Output**
```text
"cccaaa"
```

or

```text
"aaaccc"
```

---

### Example 3

**Input**
```text
s = "Aabb"
```

**Output**
```text
"bbAa"
```

or

```text
"bbaA"
```

---

# Approach 1: HashMap + PriorityQueue (Max Heap)

## Intuition

1. Count the frequency of each character using a **HashMap**.
2. Store all unique characters in a **Max Heap** based on their frequencies.
3. Remove characters one by one from the heap.
4. Append each character according to its frequency.

---

## Algorithm

1. Create a frequency map.
2. Insert every unique character into a max heap.
3. Use a custom comparator to prioritize higher frequencies.
4. Poll characters from the heap.
5. Append each character `frequency` times.

---

## Java Solution

```java
import java.util.*;

class Solution {
    public String frequencySort(String s) {

        HashMap<Character, Integer> map = new HashMap<>();

        for (char ch : s.toCharArray()) {
            map.put(ch, map.getOrDefault(ch, 0) + 1);
        }

        PriorityQueue<Character> maxHeap =
                new PriorityQueue<>((a, b) -> map.get(b) - map.get(a));

        for (char ch : map.keySet()) {
            maxHeap.offer(ch);
        }

        StringBuilder ans = new StringBuilder();

        while (!maxHeap.isEmpty()) {
            char ch = maxHeap.poll();

            for (int i = 0; i < map.get(ch); i++) {
                ans.append(ch);
            }
        }

        return ans.toString();
    }
}
```

---

## Dry Run

### Input

```text
tree
```

### Frequency Map

```text
t → 1
r → 1
e → 2
```

### Max Heap

```text
e
t
r
```

### Build Answer

```text
Poll e → "ee"

Poll t → "eet"

Poll r → "eetr"
```

Output

```text
eetr
```

---

## Complexity Analysis

### Time Complexity

- Build HashMap → **O(n)**
- Heap insertion → **O(k log k)**
- Heap removal → **O(k log k)**
- Build answer → **O(n)**

**Overall:** **O(n + k log k)**

where:

- `n` = length of string
- `k` = unique characters

---

### Space Complexity

- HashMap → **O(k)**
- PriorityQueue → **O(k)**
- StringBuilder → **O(n)**

**Overall:** **O(n + k)**

---

# Approach 2: Bucket Sort (Optimal)

## Intuition

The maximum possible frequency of a character is `n`.

Instead of sorting using a heap, create buckets where:

- Bucket index = frequency
- Bucket value = list of characters having that frequency

Traverse buckets from highest frequency to lowest.

---

## Algorithm

1. Count frequencies.
2. Create `List<Character>[] bucket`.
3. Place every character into its frequency bucket.
4. Traverse buckets from largest frequency to smallest.
5. Append each character `frequency` times.

---

## Java Solution

```java
import java.util.*;

class Solution {
    public String frequencySort(String s) {

        HashMap<Character, Integer> map = new HashMap<>();

        for (char ch : s.toCharArray()) {
            map.put(ch, map.getOrDefault(ch, 0) + 1);
        }

        List<Character>[] bucket = new ArrayList[s.length() + 1];

        for (char ch : map.keySet()) {

            int freq = map.get(ch);

            if (bucket[freq] == null) {
                bucket[freq] = new ArrayList<>();
            }

            bucket[freq].add(ch);
        }

        StringBuilder ans = new StringBuilder();

        for (int i = bucket.length - 1; i >= 1; i--) {

            if (bucket[i] != null) {

                for (char ch : bucket[i]) {

                    for (int j = 0; j < i; j++) {
                        ans.append(ch);
                    }
                }
            }
        }

        return ans.toString();
    }
}
```

---

## Dry Run

### Input

```text
tree
```

### Frequency Map

```text
t → 1
r → 1
e → 2
```

### Buckets

```text
Bucket[1] = [t, r]

Bucket[2] = [e]
```

### Traverse

```text
Bucket[2] → ee

Bucket[1] → t

Bucket[1] → r
```

Output

```text
eetr
```

---

## Complexity Analysis

### Time Complexity

- Build frequency map → **O(n)**
- Fill buckets → **O(k)**
- Traverse buckets → **O(n)**

**Overall:** **O(n)**

---

### Space Complexity

- HashMap → **O(k)**
- Bucket array → **O(n)**
- StringBuilder → **O(n)**

**Overall:** **O(n)**

---

# Comparison

| Approach | Time Complexity | Space Complexity | Interview Preference |
|-----------|-----------------|------------------|----------------------|
| HashMap + PriorityQueue | O(n + k log k) | O(k) | ⭐ Most Common |
| Bucket Sort | **O(n)** | O(n) | ⭐⭐⭐ Optimal |

---

# Key Takeaways

- Use **HashMap** to count character frequencies.
- **PriorityQueue (Max Heap)** is easy to understand and frequently asked in interviews.
- **Bucket Sort** removes the `log k` factor and achieves **O(n)** time.
- `StringBuilder` is used for efficient string construction.
- Characters with the same frequency can appear in any order.

---
