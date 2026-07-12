# Reverse and Add to Form a Palindrome

## Problem Statement

Given a positive integer `num`, repeatedly perform the following operation until the resulting number becomes a palindrome:

1. Reverse the digits of the current number.
2. Add the reversed number to the current number.

Print the final palindrome.

---

## Approach

1. Read the input number.
2. Create a function to reverse the digits of a number.
3. Create a function to check whether a number is a palindrome.
4. While the number is **not** a palindrome:
   - Reverse the number.
   - Add the reversed number to the original number.
5. Print the resulting palindrome.

---

## Algorithm

1. Input `num`.
2. Repeat until `num` becomes a palindrome:
   - `rev = reverse(num)`
   - `num = num + rev`
3. Print `num`.

---

## Java Solution

```java
import java.util.*;
import java.lang.*;
import java.io.*;

class Codechef {

    static long reverse(long num) {
        long rev = 0;

        while (num > 0) {
            rev = rev * 10 + num % 10;
            num /= 10;
        }

        return rev;
    }

    static boolean isPalindrome(long num) {
        return num == reverse(num);
    }

    public static void main(String[] args) throws java.lang.Exception {

        Scanner scan = new Scanner(System.in);
        long num = scan.nextLong();

        while (!isPalindrome(num)) {
            num += reverse(num);
        }

        System.out.print(num);
    }
}
```

---

## Example

### Input

```
195
```

### Process

| Step | Current Number | Reverse | Sum |
|------|---------------:|---------:|----:|
| 1 | 195 | 591 | 786 |
| 2 | 786 | 687 | 1473 |
| 3 | 1473 | 3741 | 5214 |
| 4 | 5214 | 4125 | 9339 ✅ |

### Output

```
9339
```

---

## Another Example

### Input

```
121
```

### Output

```
121
```

**Explanation:**  
The given number is already a palindrome, so no operations are performed.

---

## Complexity Analysis

| Operation | Complexity |
|-----------|------------|
| Reverse a number | **O(d)** |
| Palindrome check | **O(d)** |
| Overall | **O(k × d)** |

Where:

- `d` = Number of digits in the current number.
- `k` = Number of iterations required to reach a palindrome.

### Space Complexity

```
O(1)
```

Only a few variables are used, so the algorithm requires constant extra space.

---

## Key Points

- A palindrome reads the same from left to right and right to left.
- Continue adding the number and its reverse until a palindrome is obtained.
- If the input is already a palindrome, print it directly.
- The solution uses helper methods for reversing a number and checking for a palindrome, making the code clean and reusable.
