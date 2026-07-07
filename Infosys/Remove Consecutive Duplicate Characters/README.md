# Remove Consecutive Duplicate Characters

## Problem Statement

Given a string, remove only **consecutive duplicate characters** while preserving the original order of the remaining characters.

### Example

**Input**
```
AAAABBBCCDDAACC
```

**Output**
```
ABCDAC
```

### Explanation

- `"AAAA"` → `"A"`
- `"BBB"` → `"B"`
- `"CC"` → `"C"`
- `"DD"` → `"D"`
- `"AA"` → `"A"`
- `"CC"` → `"C"`

Hence, the final output is:

```
ABCDAC
```

---

## Approach

1. Create a `StringBuilder` to store the result.
2. Append the first character of the string.
3. Traverse the string from the second character onward.
4. Compare the current character with the previous character.
5. If they are different, append the current character to the result.
6. Return the final string.

---

## Algorithm

1. Read the input string.
2. Initialize a `StringBuilder`.
3. Append the first character.
4. Loop from index `1` to `n-1`:
   - If `s.charAt(i) != s.charAt(i - 1)`, append it.
5. Print the resulting string.

---

## Java Solution

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);
        String s = scan.nextLine();

        StringBuilder ans = new StringBuilder();

        ans.append(s.charAt(0));

        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(i) != s.charAt(i - 1)) {
                ans.append(s.charAt(i));
            }
        }

        System.out.println(ans.toString());
    }
}
```

---

## Dry Run

### Input

```
AAAABBBCCDDAACC
```

| Index | Current | Previous | Action | Result |
|------:|:-------:|:--------:|:------:|:------:|
| 0 | A | - | Append | A |
| 1 | A | A | Skip | A |
| 2 | A | A | Skip | A |
| 3 | A | A | Skip | A |
| 4 | B | A | Append | AB |
| 5 | B | B | Skip | AB |
| 6 | B | B | Skip | AB |
| 7 | C | B | Append | ABC |
| 8 | C | C | Skip | ABC |
| 9 | D | C | Append | ABCD |
| 10 | D | D | Skip | ABCD |
| 11 | A | D | Append | ABCDA |
| 12 | A | A | Skip | ABCDA |
| 13 | C | A | Append | ABCDAC |
| 14 | C | C | Skip | ABCDAC |

### Output

```
ABCDAC
```

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(n)` (for the output string)

---

## Key Points

- Removes **only consecutive duplicate characters**.
- Preserves the original order of characters.
- Does **not** use a `HashSet`, since a `HashSet` removes all duplicates and cannot produce outputs like `ABCDAC`.
- Efficient single-pass solution using `StringBuilder`.
