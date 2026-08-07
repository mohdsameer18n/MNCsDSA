# LeetCode 17 – Letter Combinations of a Phone Number

## Problem Statement

Given a string containing digits from **2 to 9**, return **all possible letter combinations** that the number could represent.

The mapping of digits to letters is the same as on a traditional phone keypad.

```
2 → abc
3 → def
4 → ghi
5 → jkl
6 → mno
7 → pqrs
8 → tuv
9 → wxyz
```

Return the combinations in **any order**.

---

## Example 1

### Input

```text
digits = "23"
```

### Output

```text
["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

### Explanation

```
2 → abc
3 → def

a + d = ad
a + e = ae
a + f = af

b + d = bd
b + e = be
b + f = bf

c + d = cd
c + e = ce
c + f = cf
```

---

## Example 2

### Input

```text
digits = ""
```

### Output

```text
[]
```

---

## Example 3

### Input

```text
digits = "7"
```

### Output

```text
["p","q","r","s"]
```

---

# Intuition

Each digit represents multiple letters.

For every digit:

- Choose one possible letter.
- Move to the next digit.
- Repeat until all digits are processed.

Every complete path forms one valid answer.

This is a classic **Backtracking** problem because we:

- Make a choice
- Explore recursively
- Undo the choice (Backtrack)

---

# Approach

1. Store the keypad mapping in an array.
2. Start from index `0`.
3. For the current digit:
   - Get all corresponding letters.
   - Try every letter.
4. Append the chosen letter to the current string.
5. Recurse for the next digit.
6. When all digits are processed, add the current string to the answer.
7. Remove the last letter before exploring the next option.

---

# Algorithm

```
Initialize keypad mapping.

If input is empty
    return empty list.

Call recursive function.

Recursive Function(index, currentString)

    If index == digits.length
        Store currentString
        Return

    letters = mapping[current digit]

    For every character in letters

        Append character

        Recurse for next digit

        Remove last character (Backtrack)

Return answer.
```

---

# Dry Run

## Input

```
digits = "23"
```

Initially

```
index = 0
current = ""
```

Digit

```
2 → abc
```

Choose

```
a
```

Current

```
"a"
```

Move to next digit.

```
3 → def
```

Choose

```
d
```

Current

```
"ad"
```

Reached end.

Store

```
ad
```

Backtrack

```
Remove d

Current = "a"
```

Choose

```
e
```

Store

```
ae
```

Backtrack

```
Current = "a"
```

Choose

```
f
```

Store

```
af
```

Backtrack

```
Current = ""
```

Repeat the same for

```
b

Produces

bd
be
bf
```

Then

```
c

Produces

cd
ce
cf
```

Final Answer

```
[
ad
ae
af
bd
be
bf
cd
ce
cf
]
```

---

# Recursion Tree

```
                    ""

            /        |        \
           a         b         c

        /  |  \    / | \     / | \
       d   e   f  d  e  f   d  e  f

      ad  ae  af bd be bf  cd ce cf
```

---

# Java Code

```java
import java.util.*;

class Solution {

    String[] map = {
        "", "", "abc", "def", "ghi",
        "jkl", "mno", "pqrs", "tuv", "wxyz"
    };

    List<String> ans = new ArrayList<>();

    public List<String> letterCombinations(String digits) {

        if (digits.length() == 0)
            return ans;

        backtrack(digits, 0, new StringBuilder());

        return ans;
    }

    void backtrack(String digits, int index, StringBuilder current) {

        if (index == digits.length()) {
            ans.add(current.toString());
            return;
        }

        String letters = map[digits.charAt(index) - '0'];

        for (char ch : letters.toCharArray()) {

            current.append(ch);

            backtrack(digits, index + 1, current);

            current.deleteCharAt(current.length() - 1);
        }
    }
}
```

---

# Why Backtracking?

For every digit we have multiple choices.

Example

```
2 → a,b,c
```

After trying one choice,

```
a
```

we must remove it before trying

```
b
```

This "undo" step is called **Backtracking**.

```
Choose

↓

Explore

↓

Undo

↓

Choose Next
```

---

# Complexity Analysis

Let:

- **n** = number of digits

Each digit has at most **4 letters**.

### Time Complexity

```
O(4ⁿ × n)
```

- There can be at most `4ⁿ` combinations.
- Each combination has length `n`.

---

### Space Complexity

```
O(n)
```

- Recursive call stack depth is `n`.
- (Excluding the output list.)

---

# Pattern Recognition

If a problem asks you to:

- Generate all combinations
- Generate all strings
- Generate all possibilities
- Explore every choice

Think of **Backtracking**.

---

# Similar Problems

- LeetCode 22 – Generate Parentheses
- LeetCode 39 – Combination Sum
- LeetCode 46 – Permutations
- LeetCode 47 – Permutations II
- LeetCode 77 – Combinations
- LeetCode 78 – Subsets
- LeetCode 90 – Subsets II
- LeetCode 131 – Palindrome Partitioning
- LeetCode 51 – N-Queens
- LeetCode 79 – Word Search

All of these follow the same pattern:

```
Choose
↓

Recurse
↓

Backtrack (Undo)

↓

Choose Next
```

---

# Key Takeaways

- Use recursion to process one digit at a time.
- Try every possible letter for the current digit.
- Append the letter before recursion.
- Remove the letter after recursion (Backtracking).
- Every root-to-leaf path represents one valid combination.
- This is a classic **Backtracking** interview problem frequently asked in coding interviews.
