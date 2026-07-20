# Generate Parentheses

## Problem Statement

Given `n` pairs of parentheses, write a function to generate **all combinations of well-formed (valid)** parentheses.

### Example

**Input**

```text
n = 3
```

**Output**

```text
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

---

## Approach: Backtracking

We build the answer one character at a time.

At every step:

* Add `'('` if we still have opening parentheses remaining.
* Add `')'` only if it keeps the sequence valid (i.e., the number of closing parentheses used is less than the number of opening parentheses used).

Whenever the current string reaches length `2 × n`, it is a valid combination and is added to the result.

---

## Algorithm

1. Start with an empty string.
2. Keep track of:

   * `open` = number of `'('` used.
   * `close` = number of `')'` used.
3. If `open < n`, add `'('` and recurse.
4. If `close < open`, add `')'` and recurse.
5. When the string length becomes `2 × n`, store it in the answer.

---

## Java Solution

```java
class Solution {

    public List<String> generateParenthesis(int n) {

        List<String> ans = new ArrayList<>();
        backtrack(ans, "", 0, 0, n);
        return ans;
    }

    private void backtrack(List<String> ans, String curr,
                           int open, int close, int n) {

        if (curr.length() == 2 * n) {
            ans.add(curr);
            return;
        }

        if (open < n) {
            backtrack(ans, curr + "(", open + 1, close, n);
        }

        if (close < open) {
            backtrack(ans, curr + ")", open, close + 1, n);
        }
    }
}
```

---

## Dry Run (`n = 2`)

```text
Start: ""

→ "("
   → "(("
      → "(())"
   → "()"
      → "()()"
```

**Result**

```text
[
  "(())",
  "()()"
]
```

---

## Complexity Analysis

* **Time Complexity:** `O(Cₙ × n)`

  Where `Cₙ` is the **n-th Catalan Number**, representing the number of valid combinations.

* **Space Complexity:** `O(n)`

  * Recursive call stack depth is at most `2n`.
  * Excluding the space required for storing the output.

---

## Key Concepts

* Backtracking
* Recursion
* Pruning invalid states
* Decision Tree

---

## Similar Problems

* Letter Combinations of a Phone Number
* Subsets
* Permutations
* Combination Sum
* N-Queens
* Palindrome Partitioning

---

## Takeaways

* Generate the answer incrementally.
* Never allow `close > open`.
* Stop recursion when the current string reaches length `2 × n`.
* Backtracking efficiently explores all valid combinations while pruning invalid paths early.
