# 205. Isomorphic Strings

## Problem Statement

Given two strings `s` and `t`, determine if they are **isomorphic**.

Two strings are isomorphic if the characters in `s` can be replaced to get `t`.

Rules:
- Every character in `s` must map to exactly one character in `t`.
- No two different characters in `s` can map to the same character in `t`.
- A character may map to itself.

---

## Example 1

### Input

```text
s = "egg"
t = "add"
```

### Output

```text
true
```

### Explanation

```
e → a
g → d
```

Each character has a unique mapping.

---

## Example 2

### Input

```text
s = "foo"
t = "bar"
```

### Output

```text
false
```

### Explanation

```
o → a
o → r
```

Character `o` cannot map to two different characters.

---

## Example 3

### Input

```text
s = "paper"
t = "title"
```

### Output

```text
true
```

### Explanation

```
p → t
a → i
e → l
r → e
```

All mappings are unique and consistent.

---

# Approach

To verify whether two strings are isomorphic, we maintain **two HashMaps**:

- `mapSt` → Maps characters from `s` to `t`
- `mapTs` → Maps characters from `t` to `s`

Using two maps ensures a **one-to-one mapping** between characters.

For each character pair:

1. Check if the character from `s` is already mapped.
   - If yes, verify that it maps to the current character in `t`.
   - If not, create the mapping.

2. Do the same in the reverse direction (`t → s`).

If any mapping is inconsistent, return `false`.

If all mappings remain valid, return `true`.

---

# Algorithm

1. If the lengths of the strings are different, return `false`.
2. Create two HashMaps.
3. Traverse both strings simultaneously.
4. Check the mapping from `s → t`.
5. Check the mapping from `t → s`.
6. If any mismatch occurs, return `false`.
7. Otherwise, return `true`.

---

# Java Solution

```java
class Solution {
    public boolean isIsomorphic(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        HashMap<Character, Character> mapSt = new HashMap<>();
        HashMap<Character, Character> mapTs = new HashMap<>();

        for (int i = 0; i < s.length(); i++) {

            char c1 = s.charAt(i);
            char c2 = t.charAt(i);

            if (mapSt.containsKey(c1)) {
                if (mapSt.get(c1) != c2) {
                    return false;
                }
            } else {
                mapSt.put(c1, c2);
            }

            if (mapTs.containsKey(c2)) {
                if (mapTs.get(c2) != c1) {
                    return false;
                }
            } else {
                mapTs.put(c2, c1);
            }
        }

        return true;
    }
}
```

---

# Dry Run

### Input

```text
s = "paper"
t = "title"
```

| Index | s[i] | t[i] | mapSt | mapTs |
|------:|:----:|:----:|:------|:------|
| 0 | p | t | p→t | t→p |
| 1 | a | i | a→i | i→a |
| 2 | p | t | Already mapped ✓ | Already mapped ✓ |
| 3 | e | l | e→l | l→e |
| 4 | r | e | r→e | e→r |

No conflicts are found.

**Output**

```text
true
```

---

# Why Two HashMaps?

Using only one HashMap is not sufficient.

Example:

```text
s = "ab"
t = "aa"
```

Using only:

```
a → a
b → a
```

The mapping appears valid, but two different characters (`a` and `b`) map to the same character (`a`), which is not allowed.

Using two HashMaps detects this conflict immediately.

---

# Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O(n)** |
| Space | **O(n)** |

- **Time Complexity:** `O(n)` because each character is processed once.
- **Space Complexity:** `O(n)` for storing character mappings.

---

# Key Takeaways

- Use **two HashMaps** to maintain one-to-one character mapping.
- Check mappings in both directions (`s → t` and `t → s`).
- Return `false` immediately if any mapping is inconsistent.
- Efficient solution with **O(n)** time complexity.
