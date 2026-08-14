# String Differs by Exactly One Position

## Problem Statement

Given a string `s` and an array of strings `arr[]`, determine whether there exists any string in `arr[]` that:

1. Has the **same length** as `s`.
2. Differs from `s` at **exactly one position**.

Return `true` if such a string exists; otherwise, return `false`.

## Examples

### Example 1

**Input:**

```text
arr[] = ["bana", "apple", "banaba", "bonaba"]
s = "banana"
```

**Output:**

```text
true
```

**Explanation:**

`"banana"` and `"banaba"` have the same length and differ at exactly one position.

```text
banana
banaba
     ^
```

Therefore, the answer is `true`.

### Example 2

**Input:**

```text
arr[] = ["bana", "apple", "banaba", "bonanzo"]
s = "apple"
```

**Output:**

```text
false
```

**Explanation:**

No string in the array has the same length as `"apple"` and differs from it at exactly one position.

## Approach

Use a simple character-by-character comparison.

For every string in `arr[]`:

1. Check whether its length is equal to `s.length()`.
2. If the lengths are different, skip the string.
3. Compare the characters at every position.
4. Increment `count` whenever two characters are different.
5. If `count == 1`, return `true`.
6. If no string satisfies the condition, return `false`.

### Optimization

If `count` becomes greater than `1`, we can immediately stop checking the current string because it can no longer be a valid answer.

## Java Solution

```java
class Solution {
    public boolean isStringExist(String s, String[] arr) {

        for (int i = 0; i < arr.length; i++) {

            String str = arr[i];

            // Strings must have the same length
            if (s.length() != str.length()) {
                continue;
            }

            int count = 0;

            for (int j = 0; j < s.length(); j++) {

                if (s.charAt(j) != str.charAt(j)) {
                    count++;
                }

                // More than one difference is invalid
                if (count > 1) {
                    break;
                }
            }

            if (count == 1) {
                return true;
            }
        }

        return false;
    }
}
```

## Dry Run

Consider:

```text
s = "banana"
arr = ["bana", "apple", "banaba", "bonaba"]
```

### `"bana"`

Length is different:

```text
"banana" → 6
"bana"   → 4
```

Skip it.

### `"apple"`

Length is different:

```text
"banana" → 6
"apple"  → 5
```

Skip it.

### `"banaba"`

Both have length `6`.

```text
banana
banaba
     ^
```

Comparison:

```text
b = b
a = a
n = n
a = a
n = b  → different
a = a
```

Therefore:

```text
count = 1
```

Return:

```text
true
```

## Complexity

Let:

* `n` = number of strings in `arr`
* `m` = length of `s`

### Time Complexity

```text
O(n × m)
```

In the worst case, every string has the same length as `s` and all characters need to be checked.

### Space Complexity

```text
O(1)
```

Only a few variables are used.

## Key Idea

The condition is **exactly one character difference**, not at most one.

```text
count == 1  → valid
count == 0  → same string, invalid
count > 1   → more than one difference, invalid
```
