# Maximum Distinct Elements After D Changes

## Problem

Given an integer array `arr` of size `n` and an integer `d`, we can change up to `d` duplicate elements into new values.

Find the maximum number of distinct elements possible after making at most `d` changes.

---

## Example

### Input

```text
n = 3
arr = [2, 2, 2]
d = 1
```

### Explanation

Initially:

```text
[2, 2, 2]
```

There is only **1 distinct element**:

```text
{2}
```

There are:

```text
duplicates = 3 - 1 = 2
```

We can change one duplicate `2` into a new value:

```text
[2, 2, 2] → [1, 2, 2]
```

Now the distinct elements are:

```text
{1, 2}
```

Therefore, the answer is:

```text
2
```

---

# Solution 1: Using If-Else

## Approach

1. Use a `HashSet` to find the number of distinct elements.
2. Calculate the number of duplicates:

```text
duplicates = n - distinct
```

3. If `d` is greater than or equal to the number of duplicates, all duplicates can be converted into new distinct values.
4. Otherwise, only `d` duplicates can be converted.
5. Calculate the final answer.

### Formula

```text
if d >= duplicates:
    answer = distinct + duplicates
else:
    answer = distinct + d
```

## Java Code

```java
import java.util.*;

public class Main {

    public static int solve(int[] arr, long n, long d) {

        HashSet<Integer> set = new HashSet<>();

        for (long i = 0; i < n; i++) {
            set.add(arr[(int) i]);
        }

        long distinct = set.size();
        long duplicates = n - distinct;

        long res;

        if (d >= duplicates) {
            res = distinct + duplicates;
        } else {
            res = distinct + d;
        }

        return (int) res;
    }

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

        int[] arr = new int[n];

        for (int i = 0; i < n; i++) {
            arr[i] = scan.nextInt();
        }

        long d = scan.nextLong();

        System.out.print(solve(arr, n, d));
    }
}
```

---

# Solution 2: Using Math.min()

The same logic can be simplified using `Math.min()`.

Each change can increase the number of distinct elements by at most `1`.

However, we cannot make more new distinct elements than the number of duplicates available.

Therefore:

```text
answer = distinct + min(d, duplicates)
```

## Java Code

```java
import java.util.*;

public class Main {

    public static int solve(int[] arr, long n, long d) {

        HashSet<Integer> set = new HashSet<>();

        for (long i = 0; i < n; i++) {
            set.add(arr[(int) i]);
        }

        long distinct = set.size();
        long duplicates = n - distinct;

        long res = distinct + Math.min(d, duplicates);

        return (int) res;
    }

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

        int[] arr = new int[n];

        for (int i = 0; i < n; i++) {
            arr[i] = scan.nextInt();
        }

        long d = scan.nextLong();

        System.out.print(solve(arr, n, d));
    }
}
```

---

# Comparison

| Solution   | Logic                                | Advantage          |
| ---------- | ------------------------------------ | ------------------ |
| If-Else    | Explicitly checks `d >= duplicates`  | Easy to understand |
| Math.min() | `distinct + Math.min(d, duplicates)` | Short and clean    |

Both solutions produce the same result.

---

# Complexity

For both solutions:

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(n)`

The `HashSet` stores the distinct elements.

---

# Important Notes

## Why use `HashSet<Integer>`?

The array is:

```text
int[] arr
```

Therefore, use:

```text
HashSet<Integer>
```

not:

```text
HashSet<Long>
```

## Why are `n` and `d` long?

The method is defined as:

```java
public static int solve(int[] arr, long n, long d)
```

So `n` and `d` are handled as `long`.

## Why cast the result?

The method must return `int`:

```java
public static int solve(...)
```

But `res` is `long`, so:

```java
return (int) res;
```

is required.
