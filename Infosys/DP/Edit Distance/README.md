# Edit Distance (LeetCode 72)

## Problem

Given two strings `word1` and `word2`, return the **minimum number of operations** required to convert `word1` into `word2`.

You can perform the following operations:

- Insert a character
- Delete a character
- Replace a character

---

## Example

### Input

```text
word1 = "horse"
word2 = "ros"
```

### Output

```text
3
```

### Explanation

```
horse
↓ Replace 'h' with 'r'
rorse
↓ Delete 'r'
rose
↓ Delete 'e'
ros
```

---

# Approach (Top-Down DP + Memoization)

At every position `(i, j)`:

- `i` → current index in `word1`
- `j` → current index in `word2`

We compute the minimum operations needed to convert

```
word1[i...]
```

into

```
word2[j...]
```

and store the answer in `dp[i][j]`.

---

# DP State

```
dp[i][j]
```

represents

> Minimum operations required to convert `word1[i...]` into `word2[j...]`.

---

# Base Cases

## Case 1

If `word1` is completely processed

```java
if(i == m)
    return n - j;
```

Example

```
word1 = ""
word2 = "abc"
```

Need to insert

```
a
b
c
```

Answer

```
3
```

---

## Case 2

If `word2` is completely processed

```java
if(j == n)
    return m - i;
```

Example

```
word1 = "abc"
word2 = ""
```

Need to delete

```
a
b
c
```

Answer

```
3
```

---

# Transition

## Case 1: Characters Match

```
word1[i] == word2[j]
```

Example

```
abc
abc
 ^
```

No operation is required.

Move both pointers.

```java
dp[i][j] = solve(i+1, j+1);
```

---

## Case 2: Characters Do Not Match

Example

```
ab
ac
 ^
```

Three operations are possible.

---

### 1. Insert

Insert current character of `word2`.

```
ab
ac
```

Insert `'c'`.

Move only `j`.

```java
insert = solve(i, j+1);
```

Cost

```
1 + insert
```

---

### 2. Delete

Delete current character of `word1`.

Move only `i`.

```java
delete = solve(i+1, j);
```

Cost

```
1 + delete
```

---

### 3. Replace

Replace current character.

Move both pointers.

```java
replace = solve(i+1, j+1);
```

Cost

```
1 + replace
```

---

Take the minimum.

```java
dp[i][j] =
1 + Math.min(insert,
    Math.min(delete, replace));
```

---

# Dry Run

Input

```
word1 = "ab"
word2 = "ac"
```

---

### Call

```
solve(0,0)
```

Current characters

```
a == a
```

Move both pointers.

```
solve(1,1)
```

---

### solve(1,1)

Characters

```
b != c
```

Try all operations.

### Insert

```
solve(1,2)

j == n

Return

1
```

---

### Delete

```
solve(2,1)

i == m

Return

1
```

---

### Replace

```
solve(2,2)

Both strings finished

Return

0
```

---

Take minimum

```
1 + min(1,1,0)

=1
```

Store

```
dp[1][1]=1
```

Return

```
1
```

---

Back to

```
solve(0,0)
```

Since

```
a==a
```

```
dp[0][0]=1
```

Final Answer

```
1
```

Operation

```
ab
│
Replace b → c
│
ac
```

---

# Recursion Tree

```
solve(0,0)
      |
      |
(a==a)
      |
      |
solve(1,1)
   /   |    \
  /    |     \
Insert Delete Replace
  |      |       |
(1,2)  (2,1)   (2,2)
  1      1       0

Answer

1 + min(1,1,0)

=1
```

---

# Java Solution

```java
class Solution {

    public int solve(int i, int j, String s1, String s2, Integer[][] dp) {

        int m = s1.length();
        int n = s2.length();

        if (i == m)
            return n - j;

        if (j == n)
            return m - i;

        if (dp[i][j] != null)
            return dp[i][j];

        // Characters match
        if (s1.charAt(i) == s2.charAt(j)) {
            return dp[i][j] = solve(i + 1, j + 1, s1, s2, dp);
        }

        // Characters don't match
        int insert = solve(i, j + 1, s1, s2, dp);
        int delete = solve(i + 1, j, s1, s2, dp);
        int replace = solve(i + 1, j + 1, s1, s2, dp);

        return dp[i][j] =
                1 + Math.min(insert,
                    Math.min(delete, replace));
    }

    public int minDistance(String word1, String word2) {

        Integer[][] dp =
                new Integer[word1.length()][word2.length()];

        return solve(0, 0, word1, word2, dp);
    }
}
```

---

# Complexity Analysis

### Time Complexity

```
O(m × n)
```

Each state `(i, j)` is computed only once.

---

### Space Complexity

```
O(m × n)
```

For the DP table.

Recursion stack:

```
O(m + n)
```

---

# Key Takeaways

- `dp[i][j]` stores the minimum operations to convert `word1[i...]` into `word2[j...]`.
- If characters match, move both pointers.
- If they don't match, try **Insert**, **Delete**, and **Replace**.
- Take the minimum of the three operations.
- Use memoization so each state is solved only once.
