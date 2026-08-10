# Greatest Common Divisor of Strings

**LeetCode Problem:** [Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/)

## Problem Description

For two strings `str1` and `str2`, we need to find the **largest string** that can be used to build both strings by concatenating it multiple times.

A string `x` divides a string `s` if:

```text
s = x + x + x + ... + x
```

### Example 1

```text
Input:
str1 = "ABCABC"
str2 = "ABC"

Output:
"ABC"
```

Because:

```text
"ABCABC" = "ABC" + "ABC"
"ABC"    = "ABC"
```

So `"ABC"` divides both strings.

---

## Example 2

```text
Input:
str1 = "ABABAB"
str2 = "ABAB"

Output:
"AB"
```

Because:

```text
"ABABAB" = "AB" + "AB" + "AB"
"ABAB"   = "AB" + "AB"
```

Therefore, the answer is `"AB"`.

---

## Example 3

```text
Input:
str1 = "LEET"
str2 = "CODE"

Output:
""
```

There is no common string that can construct both strings.

---

# Approach

There are two important observations.

## 1. Check if a common divisor is possible

If `str1` and `str2` have a common divisor string, then:

```text
str1 + str2 == str2 + str1
```

For example:

```text
str1 = "ABAB"
str2 = "AB"

str1 + str2 = "ABABAB"
str2 + str1 = "ABABAB"
```

They are equal, so a common divisor exists.

But:

```text
str1 = "LEET"
str2 = "CODE"

str1 + str2 = "LEETCODE"
str2 + str1 = "CODELEET"
```

They are different, so no common divisor exists.

---

## 2. Find GCD of the lengths

If a common divisor exists, the answer's length is:

```text
GCD(str1.length(), str2.length())
```

For:

```text
str1 = "ABABAB"   → length = 6
str2 = "ABAB"     → length = 4
```

Calculate:

```text
GCD(6, 4) = 2
```

The first `2` characters are:

```text
"AB"
```

So the answer is:

```text
"AB"
```

---

# Algorithm

```text
1. Check:
   str1 + str2 == str2 + str1

2. If they are not equal:
      return ""

3. Find:
      gcd(str1.length(), str2.length())

4. Return the substring of str1
   from index 0 to gcd.
```

---

# Java Solution

```java
class Solution {

    public String gcdOfStrings(String str1, String str2) {

        // If strings cannot have a common divisor
        if (!(str1 + str2).equals(str2 + str1)) {
            return "";
        }

        int gcdLength = gcd(str1.length(), str2.length());

        return str1.substring(0, gcdLength);
    }

    private int gcd(int a, int b) {

        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }

        return a;
    }
}
```

---

# Dry Run

### Input

```text
str1 = "ABCABC"
str2 = "ABC"
```

### Step 1: Concatenate

```text
str1 + str2
= "ABCABCABC"

str2 + str1
= "ABCABCABC"
```

They are equal.

So a common divisor exists.

---

### Step 2: Find lengths

```text
str1.length() = 6
str2.length() = 3
```

Find:

```text
GCD(6, 3)
```

Using Euclid's Algorithm:

```text
6 % 3 = 0
```

Therefore:

```text
GCD = 3
```

---

### Step 3: Take first 3 characters

```text
str1 = "ABCABC"
         ↑↑↑
         ABC
```

Therefore:

```text
Output = "ABC"
```

---

# Another Dry Run

### Input

```text
str1 = "ABABAB"
str2 = "ABAB"
```

Lengths:

```text
6 and 4
```

GCD:

```text
6 % 4 = 2
4 % 2 = 0

GCD = 2
```

Take first `2` characters:

```text
"AB"
```

Output:

```text
"AB"
```

---

# Why Does `str1 + str2 == str2 + str1` Work?

Suppose a common divisor exists:

```text
X = "AB"
```

Then:

```text
str1 = X + X + X
str2 = X + X
```

Therefore:

```text
str1 + str2
= X + X + X + X + X

str2 + str1
= X + X + X + X + X
```

Both are identical.

If:

```text
str1 + str2 != str2 + str1
```

then the two strings cannot be constructed from the same repeating base string.

---

# Complexity Analysis

Let `n` and `m` be the lengths of `str1` and `str2`.

### Time Complexity

String concatenation and comparison:

```text
O(n + m)
```

GCD of lengths:

```text
O(log(min(n, m)))
```

Overall:

```text
O(n + m)
```

### Space Complexity

The concatenated strings require:

```text
O(n + m)
```

Additional algorithmic space apart from the temporary concatenated strings is:

```text
O(1)
```

---

# Key Takeaways

* First check whether the strings have the same repeating pattern.
* Use:

```java
(str1 + str2).equals(str2 + str1)
```

* If they match, calculate:

```text
GCD(str1.length(), str2.length())
```

* The answer is the prefix of `str1` having that GCD length.

### Pattern

```text
String GCD
     ↓
Check concatenation
     ↓
str1 + str2 == str2 + str1
     ↓
Find GCD of lengths
     ↓
Take prefix of GCD length
```
