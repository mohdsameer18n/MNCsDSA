# String Palindrome, Vowel and Consonant Count

## Problem

Given a string, perform the following operations:

1. Check whether the string is a **palindrome**.
2. Count the number of **consonants**.
3. Count the number of **vowels**.
4. Print the results in the required order.

The string is converted to lowercase before performing the checks.

---

## Example

### Input

```text
5
Madam
```

### Explanation

The string is:

```text
Madam
```

After converting to lowercase:

```text
madam
```

It reads the same from both directions, so it is a palindrome.

Vowels:

```text
a, a
```

Total vowels:

```text
2
```

Consonants:

```text
m, d, m
```

Total consonants:

```text
3
```

### Output

```text
true
3
2
```

The output order is:

```text
Palindrome
Consonants
Vowels
```

---

# Approach

## 1. Convert String to Lowercase

```java
String str = s.toLowerCase();
```

This makes the palindrome check case-insensitive.

For example:

```text
Madam → madam
```

---

## 2. Check Palindrome

Use the **two-pointer technique**.

* `left` starts from the beginning.
* `right` starts from the end.
* Compare both characters.
* Move `left` forward and `right` backward.
* If any pair is different, the string is not a palindrome.

```java
boolean palindrome = true;

int left = 0;
int right = str.length() - 1;

while (left < right) {

    if (str.charAt(left) != str.charAt(right)) {
        palindrome = false;
        break;
    }

    left++;
    right--;
}
```

### Example

```text
madam

m == m
a == a
d == d
```

Therefore:

```text
true
```

---

# 3. Count Vowels and Consonants

Loop through every character in the string.

Vowels are:

```text
a, e, i, o, u
```

If the character is a vowel, increment `vowels`.

Otherwise, increment `consonants`.

```java
int vowels = 0;
int consonants = 0;

for (char ch : str.toCharArray()) {

    if (ch == 'a' || ch == 'e' || ch == 'i' ||
        ch == 'o' || ch == 'u') {

        vowels++;

    } else {
        consonants++;
    }
}
```

---

# Complete Java Code

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();
        String s = sc.next();

        String str = s.toLowerCase();

        // Palindrome check
        boolean palindrome = true;

        int left = 0;
        int right = str.length() - 1;

        while (left < right) {

            if (str.charAt(left) != str.charAt(right)) {
                palindrome = false;
                break;
            }

            left++;
            right--;
        }

        // Count vowels and consonants
        int vowels = 0;
        int consonants = 0;

        for (char ch : str.toCharArray()) {

            if (ch == 'a' || ch == 'e' || ch == 'i' ||
                ch == 'o' || ch == 'u') {

                vowels++;

            } else {
                consonants++;
            }
        }

        // Required output order
        System.out.println(palindrome);
        System.out.println(consonants);
        System.out.println(vowels);
    }
}
```

---

# Complexity Analysis

### Time Complexity

```text
O(n)
```

The string is traversed for the palindrome check and vowel/consonant counting.

### Space Complexity

```text
O(n)
```

Because `toLowerCase()` and `toCharArray()` may create additional string/character storage.

---

# Important Note

The variable `n` is read from the input:

```java
int n = sc.nextInt();
```

but the program uses the actual string length:

```java
str.length()
```

for the palindrome check.

Therefore, the input `n` should normally represent the length of the given string.
