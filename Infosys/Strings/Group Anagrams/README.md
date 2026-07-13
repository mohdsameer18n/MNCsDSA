# Group Anagrams

## Problem Statement

Given an array of strings `strs`, group the anagrams together. You can return the answer in **any order**.

An **anagram** is a word or phrase formed by rearranging the letters of another word, using all the original letters exactly once.

---

## Example 1

### Input

```text
strs = ["eat","tea","tan","ate","nat","bat"]
```

### Output

```text
[
  ["eat","tea","ate"],
  ["tan","nat"],
  ["bat"]
]
```

### Explanation

- `"eat"`, `"tea"`, and `"ate"` are anagrams because they contain the same letters.
- `"tan"` and `"nat"` are anagrams.
- `"bat"` has no anagram, so it forms its own group.

---

## Example 2

### Input

```text
strs = [""]
```

### Output

```text
[
  [""]
]
```

---

## Example 3

### Input

```text
strs = ["a"]
```

### Output

```text
[
  ["a"]
]
```

---

# Approach 1: Sorting + HashMap

## Idea

- Sort every string alphabetically.
- Use the sorted string as the key.
- Words with the same sorted key belong to the same group.
- Store them in a `HashMap<String, List<String>>`.

---

## Algorithm

1. Create a `HashMap<String, List<String>>`.
2. Traverse every word in the array.
3. Convert the word into a character array.
4. Sort the character array.
5. Convert it back into a string (key).
6. If the key is not present, create a new list.
7. Add the original word to the corresponding list.
8. Return all the grouped lists.

---

## Dry Run

### Input

```text
["eat","tea","tan","ate","nat","bat"]
```

| Word | Sorted Key | HashMap |
|------|------------|----------|
| eat | aet | aet → [eat] |
| tea | aet | aet → [eat, tea] |
| tan | ant | ant → [tan] |
| ate | aet | aet → [eat, tea, ate] |
| nat | ant | ant → [tan, nat] |
| bat | abt | abt → [bat] |

### Final Output

```text
[
  [eat, tea, ate],
  [tan, nat],
  [bat]
]
```

---

## Java Solution

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {

        HashMap<String, List<String>> map = new HashMap<>();

        for (String word : strs) {

            char[] ch = word.toCharArray();
            Arrays.sort(ch);

            String key = new String(ch);

            map.putIfAbsent(key, new ArrayList<>());
            map.get(key).add(word);
        }

        return new ArrayList<>(map.values());
    }
}
```

---

## Complexity Analysis

- **Time Complexity:** `O(N × K log K)`
  - `N` = Number of strings
  - `K` = Average length of each string

- **Space Complexity:** `O(N × K)`

---

# Approach 2: Character Frequency + HashMap

## Idea

Instead of sorting every string, count the frequency of each character.

Words having the same character frequencies are anagrams.

Example:

```text
eat

a = 1
e = 1
t = 1
others = 0
```

Generate a unique key from the frequency array and use it in the HashMap.

---

## Algorithm

1. Create a `HashMap<String, List<String>>`.
2. For every word:
   - Create an integer array of size 26.
   - Count the frequency of each character.
   - Build a unique key using the frequency array.
3. Insert the word into the corresponding list.
4. Return all grouped lists.

---

## Java Solution

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {

        HashMap<String, List<String>> map = new HashMap<>();

        for (String word : strs) {

            int[] count = new int[26];

            for (char c : word.toCharArray()) {
                count[c - 'a']++;
            }

            StringBuilder key = new StringBuilder();

            for (int num : count) {
                key.append('#').append(num);
            }

            map.putIfAbsent(key.toString(), new ArrayList<>());
            map.get(key.toString()).add(word);
        }

        return new ArrayList<>(map.values());
    }
}
```

---

## Complexity Analysis

- **Time Complexity:** `O(N × K)`
- **Space Complexity:** `O(N × K)`

---

# Comparison

| Approach | Time Complexity | Space Complexity | Advantages |
|-----------|-----------------|------------------|------------|
| Sorting + HashMap | `O(N × K log K)` | `O(N × K)` | Easy to understand and implement |
| Character Frequency + HashMap | `O(N × K)` | `O(N × K)` | Faster as it avoids sorting |

---

# Key Takeaways

- A `HashMap` is used to group strings with the same key.
- In the sorting approach, the **sorted string** is the key.
- In the frequency approach, the **character count array** is converted into a unique key.
- `putIfAbsent()` creates a new list only when the key is not already present.
- Finally, return all grouped lists using:

```java
return new ArrayList<>(map.values());
```
