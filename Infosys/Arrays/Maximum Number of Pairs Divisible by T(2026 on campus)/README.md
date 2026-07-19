# Maximum Number of Pairs Divisible by T

## Problem Statement

Given an array of `N` integers and an integer `T`, find the **maximum number of disjoint pairs** such that the **sum of each pair is divisible by `T`**.

Each element can be used **at most once**.

---

## Example 1

### Input
```text
A = [2, 4, 6, 8]
T = 2
```

### Output
```text
2
```

### Explanation

Possible pairs:

- (2, 4) → 6 → divisible by 2 ✅
- (6, 8) → 14 → divisible by 2 ✅

Maximum pairs = **2**

---

## Example 2

### Input

```text
A = [1, 2, 3, 4, 5, 6]
T = 5
```

### Output

```text
2
```

### Explanation

Valid pairs whose sum is divisible by 5:

- (1, 4)
- (2, 3)
- (4, 6)

Since element **4** cannot be used twice, the maximum number of disjoint pairs is:

- (1, 4)
- (2, 3)

Answer = **2**

---

# Approach

Instead of checking every pair, use **remainders**.

For each number:

```text
remainder = number % T
```

Two numbers form a valid pair if:

```text
(remainder1 + remainder2) % T == 0
```

This means:

```text
remainder2 = T - remainder1
```

Store the frequency of each remainder and greedily form pairs.

---

# Algorithm

1. Create a frequency array of size `T`.
2. Count the remainder of every element.
3. Pair remainder `0` with itself.
4. For every remainder `i` from `1` to `T/2`:
   - If `i == T - i` (only when `T` is even), pair elements having the same remainder.
   - Otherwise, pair remainder `i` with remainder `T - i`.
5. Return the total number of pairs.

---

# Java Solution

```java
import java.util.*;

public class Main {

    public static int solve(int[] arr, int T) {

        int[] rem = new int[T];

        // Count frequencies of remainders
        for (int num : arr) {
            rem[num % T]++;
        }

        int pair = 0;

        // Pair remainder 0 with itself
        pair += rem[0] / 2;

        // Pair remaining remainders
        for (int i = 1; i <= T / 2; i++) {

            if (i == T - i) {
                pair += rem[i] / 2;
            } else {
                pair += Math.min(rem[i], rem[T - i]);
            }
        }

        return pair;
    }

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

        int[] arr = new int[n];

        for (int i = 0; i < n; i++) {
            arr[i] = scan.nextInt();
        }

        int T = scan.nextInt();

        System.out.println(solve(arr, T));
    }
}
```

---

# Dry Run

### Input

```text
A = [1,2,3,4,5,6]
T = 5
```

### Step 1: Compute Remainders

| Number | Remainder |
|--------|-----------|
|1|1|
|2|2|
|3|3|
|4|4|
|5|0|
|6|1|

Frequency array:

|Remainder|Count|
|---------|-----|
|0|1|
|1|2|
|2|1|
|3|1|
|4|1|

### Step 2: Form Pairs

- Remainder `0`:
  - `1 / 2 = 0` pairs

- Remainder `1` with `4`
  - `min(2,1) = 1`

- Remainder `2` with `3`
  - `min(1,1) = 1`

Total pairs:

```text
0 + 1 + 1 = 2
```

---

# Complexity Analysis

- **Time Complexity:** `O(N + T)`
- **Space Complexity:** `O(T)`

---

# Key Observations

- Numbers are grouped based on their remainder modulo `T`.
- Remainder `0` pairs only with remainder `0`.
- If `T` is even, remainder `T/2` pairs only with itself.
- All other remainders `i` pair with `T - i`.
- Using remainder frequencies avoids checking every possible pair, making the solution efficient.
