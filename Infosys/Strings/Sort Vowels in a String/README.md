# LeetCode 2785 – Sort Vowels in a String

## Problem Statement

Given a **0-indexed** string `s`, sort **only the vowels** in the string in **non-decreasing ASCII order** while keeping all **non-vowel characters in their original positions**.

Return the resulting string.

A vowel is one of:

```text
A, E, I, O, U, a, e, i, o, u
```

---

## Example 1

### Input

```text
s = "lEetcOde"
```

### Output

```text
lEOtcede
```

### Explanation

Vowels in the string:

```text
E e O e
```

After sorting (ASCII order):

```text
E O e e
```

Replace them back into their original vowel positions.

Result:

```text
l E O t c e d e
```

---

## Example 2

### Input

```text
s = "lYmpH"
```

### Output

```text
lYmpH
```

There are no vowels, so the string remains unchanged.

---

# 💡 Intuition

The positions of consonants **must not change**.

Only vowels need to be sorted.

Instead of sorting the entire string:

1. Extract all vowels.
2. Sort the vowels.
3. Traverse the string again.
4. Replace only vowel positions with the sorted vowels.

This keeps consonants fixed while sorting only the vowels.

---

# 🚀 Approach

### Step 1

Create a list to store vowels.

```java
List<Character> vowels = new ArrayList<>();
```

---

### Step 2

Traverse the string.

Whenever a vowel is found,

store it in the list.

Example

```text
l E e t c O d e

Collected vowels:

[E, e, O, e]
```

---

### Step 3

Sort the vowel list.

```java
Collections.sort(vowels);
```

Sorted vowels

```text
[E, O, e, e]
```

---

### Step 4

Traverse the string again.

If the current character is:

- vowel → insert the next sorted vowel.
- consonant → keep it unchanged.

Build the answer using `StringBuilder`.

---

# 🧪 Dry Run

Input

```text
s = "lEetcOde"
```

### Step 1

Collect vowels

```text
[E, e, O, e]
```

---

### Step 2

Sort

```text
[E, O, e, e]
```

---

### Step 3

Build answer

| Character | Action | Result |
|-----------|--------|--------|
| l | Consonant | l |
| E | Replace with E | lE |
| e | Replace with O | lEO |
| t | Keep | lEOt |
| c | Keep | lEOtc |
| O | Replace with e | lEOtce |
| d | Keep | lEOtced |
| e | Replace with e | lEOtcede |

Final Answer

```text
lEOtcede
```

---

# ✅ Java Solution

```java
class Solution {

    public String sortVowels(String s) {

        List<Character> vowels = new ArrayList<>();

        // Collect all vowels
        for(char c : s.toCharArray()) {
            if(isVowel(c))
                vowels.add(c);
        }

        // Sort vowels
        Collections.sort(vowels);

        StringBuilder ans = new StringBuilder();

        int idx = 0;

        // Replace only vowel positions
        for(char c : s.toCharArray()) {

            if(isVowel(c))
                ans.append(vowels.get(idx++));
            else
                ans.append(c);
        }

        return ans.toString();
    }

    boolean isVowel(char c){
        return "AEIOUaeiou".indexOf(c) != -1;
    }
}
```

---

# 🔍 How `isVowel()` Works

```java
boolean isVowel(char c){
    return "AEIOUaeiou".indexOf(c) != -1;
}
```

The string

```text
AEIOUaeiou
```

contains all uppercase and lowercase vowels.

`indexOf(c)` returns:

- Index of the character if found.
- `-1` if not found.

Example

```java
indexOf('A') = 0
indexOf('e') = 6
indexOf('x') = -1
```

So,

```java
return indexOf(c) != -1;
```

returns:

- `true` → vowel
- `false` → consonant

---

# ✅ Correctness

- Every vowel is collected exactly once.
- Sorting arranges vowels in ascending ASCII order.
- Consonants are never modified.
- Every vowel position receives the next smallest vowel.

Thus, the final string satisfies the problem requirements.

---

# ⏱ Complexity Analysis

Let **n** be the length of the string.

### Time Complexity

Collect vowels:

```
O(n)
```

Sort vowels:

```
O(k log k)
```

where `k` is the number of vowels.

Build answer:

```
O(n)
```

Overall:

```
O(n + k log k)
```

Worst case (`k = n`):

```
O(n log n)
```

---

### Space Complexity

Vowel list:

```
O(k)
```

Answer string:

```
O(n)
```

Overall:

```
O(n)
```

---

# 🧠 Pattern Recognition

This problem follows the pattern:

> **Collect → Sort → Replace**

Use this approach whenever:

- Only certain characters/elements need to be sorted.
- Their original positions must be preserved.
- The remaining elements stay fixed.

Similar problems:

- Sort Even and Odd Indices Independently
- Rearrange Characters with Fixed Positions
- Custom String Sorting
- Sort Characters By Frequency (different sorting strategy)

---

# 🔑 Key Takeaways

- Do **not** sort the entire string.
- Extract only the characters that need sorting.
- Sort them separately.
- Reinsert them into their original positions.
- `StringBuilder` helps build the answer efficiently.
