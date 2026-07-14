# 1768. Merge Strings Alternately

**Difficulty:** Easy  
**Platform:** LeetCode

---

# Problem Statement

You are given two strings `word1` and `word2`.

Merge the strings by adding letters in alternating order, starting with `word1`.

If one string is longer than the other, append the remaining letters to the end of the merged string.

Return the merged string.

---

## Example 1

**Input**

```text
word1 = "abc"
word2 = "pqr"
```

**Output**

```text
"apbqcr"
```

**Explanation**

```text
a + p + b + q + c + r
```

---

## Example 2

**Input**

```text
word1 = "ab"
word2 = "pqrs"
```

**Output**

```text
"apbqrs"
```

**Explanation**

```text
a + p + b + q + rs
```

---

## Example 3

**Input**

```text
word1 = "abcd"
word2 = "pq"
```

**Output**

```text
"apbqcd"
```

**Explanation**

```text
a + p + b + q + cd
```

---

# Approach 1: Using String Concatenation

## Idea

- Traverse both strings simultaneously.
- Append one character from each string alternately.
- After one string ends, append the remaining characters of the other string.

---

## Algorithm

1. Initialize an empty string.
2. Traverse both strings while both have characters.
3. Append one character from each string.
4. Append remaining characters of the longer string.
5. Return the merged string.

---

## Java Solution

```java
class Solution {
    public String mergeAlternately(String word1, String word2) {

        int a = word1.length(), b = word2.length();
        String s = "";

        while (a > 0 && b > 0) {
            s += word1.charAt(word1.length() - a);
            s += word2.charAt(word2.length() - b);
            a--;
            b--;
        }

        while (a > 0) {
            s += word1.charAt(word1.length() - a);
            a--;
        }

        while (b > 0) {
            s += word2.charAt(word2.length() - b);
            b--;
        }

        return s;
    }
}
```

---

## Dry Run

### Input

```text
word1 = "abc"
word2 = "pqr"
```

| Step | Result |
|------|--------|
| Add a | a |
| Add p | ap |
| Add b | apb |
| Add q | apbq |
| Add c | apbqc |
| Add r | apbqcr |

**Output**

```text
apbqcr
```

---

## Complexity Analysis

| Complexity | Value |
|------------|-------|
| Time | **O((m+n)²)** |
| Space | **O(m+n)** |

**Reason:** Every `+=` creates a new String object.

---

# Approach 2: Using StringBuilder (Optimal)

## Idea

Instead of creating a new string every time, use `StringBuilder`, which allows efficient appending.

---

## Algorithm

1. Create a `StringBuilder`.
2. Traverse both strings together.
3. Append alternating characters.
4. Append remaining characters.
5. Convert `StringBuilder` to String.

---

## Java Solution

```java
class Solution {
    public String mergeAlternately(String word1, String word2) {

        int i = 0, j = 0;
        StringBuilder sb = new StringBuilder();

        while (i < word1.length() && j < word2.length()) {
            sb.append(word1.charAt(i++));
            sb.append(word2.charAt(j++));
        }

        while (i < word1.length()) {
            sb.append(word1.charAt(i++));
        }

        while (j < word2.length()) {
            sb.append(word2.charAt(j++));
        }

        return sb.toString();
    }
}
```

---

## Dry Run

### Input

```text
word1 = "ab"
word2 = "pqrs"
```

| Step | StringBuilder |
|------|---------------|
| Add a | a |
| Add p | ap |
| Add b | apb |
| Add q | apbq |
| Remaining | apbqrs |

**Output**

```text
apbqrs
```

---

# Why StringBuilder is Better

| String (`+=`) | StringBuilder |
|---------------|---------------|
| Immutable | Mutable |
| Creates new object on every append | Modifies same object |
| Slower | Faster |
| O((m+n)²) | O(m+n) |

---

# Complexity Comparison

| Approach | Time | Space |
|----------|------|-------|
| String Concatenation | **O((m+n)²)** | **O(m+n)** |
| StringBuilder | **O(m+n)** | **O(m+n)** |

---

# Key Takeaways

- Merge characters alternately from both strings.
- Append the remaining characters after one string ends.
- `StringBuilder` is the preferred solution because it performs efficient appends without creating new String objects.
- This is the standard interview and LeetCode solution.
