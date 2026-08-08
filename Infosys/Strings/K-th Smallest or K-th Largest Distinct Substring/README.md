# K-th Smallest / K-th Largest Distinct Substring

## Problem Statement

Given a string `str` and an integer `k`, find either:

1. The **k-th smallest distinct substring** in lexicographical order.
2. The **k-th largest distinct substring** in lexicographical order.

If there are fewer than `k` distinct substrings, return:

```text
-1
```

---

# Example

Given:

```text
str = "abc"
```

All distinct substrings are:

```text
a
ab
abc
b
bc
c
```

They are already in lexicographical order:

```text
1 → a
2 → ab
3 → abc
4 → b
5 → bc
6 → c
```

---

## K-th Smallest

If:

```text
k = 3
```

then:

```text
Output = abc
```

---

## K-th Largest

Largest order is:

```text
1 → c
2 → bc
3 → b
4 → abc
5 → ab
6 → a
```

If:

```text
k = 3
```

then:

```text
Output = b
```

---

# Common Approach

Both problems use the same substring-generation technique.

```text
String
   ↓
Generate all substrings
   ↓
TreeSet
   ↓
Remove duplicates
   ↓
Lexicographically sorted
   ↓
Find required position
```

---

# Why TreeSet?

Java's:

```java
TreeSet<String>
```

automatically provides two useful properties:

### 1. Removes duplicates

For:

```text
aaa
```

generated substrings are:

```text
a
aa
aaa
a
aa
a
```

TreeSet stores:

```text
a
aa
aaa
```

### 2. Maintains sorted order

The strings are automatically stored in lexicographical order.

---

# Generate All Substrings

The common code is:

```java
for (int i = 0; i < n; i++) {

    StringBuilder sb = new StringBuilder();

    for (int j = i; j < n; j++) {

        sb.append(str.charAt(j));

        set.add(sb.toString());
    }
}
```

For:

```text
abc
```

it generates:

```text
i = 0

a
ab
abc

i = 1

b
bc

i = 2

c
```

---

# Version 1 – K-th Smallest Distinct Substring

## Logic

Since `TreeSet` stores elements in ascending lexicographical order:

```text
a
ab
abc
b
bc
c
```

we simply take the `k-th` element.

---

## Java Code

```java
import java.util.*;

public class Main {

    public static String solve(String str, int k) {

        int n = str.length();

        TreeSet<String> set = new TreeSet<>();

        // Generate all distinct substrings
        for (int i = 0; i < n; i++) {

            StringBuilder sb = new StringBuilder();

            for (int j = i; j < n; j++) {

                sb.append(str.charAt(j));

                set.add(sb.toString());
            }
        }

        // Invalid k
        if (k <= 0 || k > set.size()) {
            return "-1";
        }

        int count = 1;

        // TreeSet is already sorted
        for (String st : set) {

            if (count == k) {
                return st;
            }

            count++;
        }

        return "-1";
    }

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        String str = scan.nextLine();

        int k = scan.nextInt();

        System.out.print(solve(str, k));
    }
}
```

---

# Dry Run – K-th Smallest

### Input

```text
abc
4
```

Generated distinct substrings:

```text
a
ab
abc
b
bc
c
```

TreeSet order:

```text
1 → a
2 → ab
3 → abc
4 → b
5 → bc
6 → c
```

We need:

```text
k = 4
```

Therefore:

```text
Output = b
```

---

# Version 2 – K-th Largest Distinct Substring

## Logic

The TreeSet is sorted from smallest to largest:

```text
a
ab
abc
b
bc
c
```

But we want the `k-th largest`.

Instead of changing the TreeSet, convert the position.

## Formula

```text
k-th largest
=
(size - k + 1)-th smallest
```

---

## Example

Suppose:

```text
size = 6
k = 2
```

Then:

```text
position = 6 - 2 + 1
         = 5
```

So we take the 5th smallest:

```text
bc
```

Therefore:

```text
2nd largest = bc
```

---

## Java Code

```java
import java.util.*;

public class Main {

    public static String solve(String str, int k) {

        int n = str.length();

        TreeSet<String> set = new TreeSet<>();

        // Generate all distinct substrings
        for (int i = 0; i < n; i++) {

            StringBuilder sb = new StringBuilder();

            for (int j = i; j < n; j++) {

                sb.append(str.charAt(j));

                set.add(sb.toString());
            }
        }

        // Invalid k
        if (k <= 0 || k > set.size()) {
            return "-1";
        }

        // Convert k-th largest
        // into position from the beginning
        int position = set.size() - k + 1;

        int count = 1;

        for (String st : set) {

            if (count == position) {
                return st;
            }

            count++;
        }

        return "-1";
    }

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        String str = scan.nextLine();

        int k = scan.nextInt();

        System.out.print(solve(str, k));
    }
}
```

---

# Dry Run – K-th Largest

### Input

```text
abc
3
```

TreeSet:

```text
1 → a
2 → ab
3 → abc
4 → b
5 → bc
6 → c
```

There are:

```text
6
```

distinct substrings.

We want:

```text
3rd largest
```

Calculate:

```text
position = size - k + 1

         = 6 - 3 + 1

         = 4
```

Take the 4th smallest:

```text
b
```

Therefore:

```text
Output = b
```

---

# Both Problems Compared

| Requirement | Position to Find |
|---|---|
| 1st smallest | `1` |
| 2nd smallest | `2` |
| k-th smallest | `k` |
| 1st largest | `size` |
| 2nd largest | `size - 1` |
| k-th largest | `size - k + 1` |

---

# Example Comparison

For:

```text
str = "abc"
```

Sorted distinct substrings:

```text
a
ab
abc
b
bc
c
```

### 2nd Smallest

```text
k = 2

Output = ab
```

### 2nd Largest

```text
k = 2

position = 6 - 2 + 1
         = 5

Output = bc
```

---

# Why StringBuilder?

We use:

```java
StringBuilder sb = new StringBuilder();
```

and:

```java
sb.append(str.charAt(j));
```

Instead of creating every substring from scratch.

For example:

```text
i = 0

""
 ↓
"a"
 ↓
"ab"
 ↓
"abc"
```

This builds each substring incrementally.

---

# Why Do We Need `TreeSet`?

If we used:

```java
ArrayList<String>
```

we would have:

```text
Duplicates
+
Need to sort manually
```

With:

```java
TreeSet<String>
```

we automatically get:

```text
Unique
+
Sorted
```

---

# Complexity Analysis

There can be:

```text
O(n²)
```

possible substrings.

Each substring can have length up to:

```text
O(n)
```

and inserting into a `TreeSet` requires comparisons.

Therefore, a practical worst-case analysis is approximately:

```text
Time:  O(n³ log n)
Space: O(n³)
```

when accounting for the characters stored across all distinct substrings.

For small or moderate constraints, this is acceptable and much easier to implement than advanced suffix-based algorithms.

---

# For Large Constraints

If:

```text
n
```

is very large, generating all substrings is not feasible.

For example, if:

```text
n = 100000
```

there can be roughly:

```text
n × (n + 1) / 2
```

possible substrings.

That is approximately:

```text
5 × 10⁹
```

substrings.

For such constraints, use advanced string algorithms such as:

```text
Suffix Array + LCP
```

or:

```text
Suffix Automaton
```

---

# Infosys Coding Drive Recommendation

For an Infosys coding round, first check the constraints.

### Small / Moderate `n`

Use:

```text
Nested Loops
+
StringBuilder
+
TreeSet
```

Advantages:

```text
Easy to remember
Easy to code
Easy to debug
Easy to explain
```

### Very Large `n`

Use:

```text
Suffix Array + LCP
```

---

# Interview Explanation

You can explain the solution like this:

> "First, I generate every possible substring using two nested loops. I use a StringBuilder to construct each substring incrementally. I insert every substring into a TreeSet, which automatically removes duplicates and maintains lexicographical order. For the k-th smallest substring, I directly take the k-th element. For the k-th largest substring, I convert its position using `size - k + 1` and take that position from the sorted TreeSet."

---

# Core Pattern

```text
                String
                   |
                   ↓
          Generate Substrings
                   |
                   ↓
              TreeSet
                   |
          +--------+--------+
          |                 |
     Unique + Sorted        |
          |                 |
          ↓                 ↓
 K-th Smallest        K-th Largest
          |                 |
          ↓                 ↓
          k          size - k + 1
```

---

# Key Takeaways

### Generate substrings

```java
for (int i = 0; i < n; i++) {

    StringBuilder sb = new StringBuilder();

    for (int j = i; j < n; j++) {

        sb.append(str.charAt(j));

        set.add(sb.toString());
    }
}
```

### K-th smallest

```java
if (count == k)
    return st;
```

### K-th largest

```java
int position = set.size() - k + 1;

if (count == position)
    return st;
```

### Invalid `k`

```java
if (k <= 0 || k > set.size())
    return "-1";
```

---

# Final Pattern to Remember

```text
Generate all substrings
        ↓
Store in TreeSet
        ↓
Duplicates removed
        ↓
Lexicographical sorting
        ↓
       / \
      /   \
     ↓     ↓
Smallest  Largest
   ↓         ↓
   k     size-k+1
```

Both problems use **exactly the same substring-generation code**. Only the final selection logic changes.
