# Longest Valid Parentheses

## Problem Statement

Given a string `s` containing only `'('` and `')'`, find the length of the **longest valid (well-formed) parentheses substring**.

### Example 1

**Input**

```text
s = "(()"
```

**Output**

```text
2
```

**Explanation:** The longest valid substring is `"()"`.

### Example 2

**Input**

```text
s = ")()())"
```

**Output**

```text
4
```

**Explanation:** The longest valid substring is `"()()"`.

### Example 3

**Input**

```text
s = ""
```

**Output**

```text
0
```

## Approach

We use a **Stack** to store indices.

Initially, push `-1` into the stack. This acts as a base index for calculating the length of valid parentheses.

### Algorithm

1. Traverse the string from left to right.
2. If the current character is `'('`, push its index into the stack.
3. If the current character is `')'`:

   * Pop the top element.
   * If the stack becomes empty, push the current index as a new starting boundary.
   * Otherwise, calculate the current valid substring length:

     ```java
     i - stack.peek()
     ```
   * Update `max`.

## Java Solution

```java
import java.util.Stack;

class Solution {
    public int longestValidParentheses(String s) {

        Stack<Integer> stack = new Stack<>();
        stack.push(-1);

        int max = 0;

        for (int i = 0; i < s.length(); i++) {

            if (s.charAt(i) == '(') {
                stack.push(i);
            } 
            else {
                stack.pop();

                if (stack.isEmpty()) {
                    stack.push(i);
                } 
                else {
                    max = Math.max(max, i - stack.peek());
                }
            }
        }

        return max;
    }
}
```

## Dry Run

For:

```text
s = ")()())"
```

| Index | Character | Stack       | Max |
| ----: | :-------: | :---------- | --: |
|     0 |    `)`    | `[0]`       |   0 |
|     1 |    `(`    | `[0, 1]`    |   0 |
|     2 |    `)`    | `[0, 2]`    |   2 |
|     3 |    `(`    | `[0, 2, 3]` |   2 |
|     4 |    `)`    | `[0, 2, 4]` |   4 |
|     5 |    `)`    | `[5]`       |   4 |

Therefore:

```text
Answer = 4
```

## Why `-1` Is Used

The initial:

```java
stack.push(-1);
```

provides a boundary before the string starts.

For example:

```text
s = "()"
```

At index `1`:

```text
i - stack.peek()
= 1 - (-1)
= 2
```

So the length of `"()"` is correctly calculated as `2`.

## Complexity

* **Time:** `O(n)` — each character is processed once.
* **Space:** `O(n)` — the stack can contain indices for all `'('` characters.

## Key Idea

The stack stores **indices**, not parentheses characters.

The top of the stack represents the index immediately before the current valid substring, so:

```java
i - stack.peek()
```

gives the length of the current valid parentheses substring.
