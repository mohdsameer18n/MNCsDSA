# Count Primes

## Problem Statement

Given an integer `n`, return the number of prime numbers that are **strictly less than** `n`.

A **prime number** is a natural number greater than `1` that has exactly two positive divisors: `1` and itself.

---

## Examples

### Example 1

**Input**
```text
n = 10
```

**Output**
```text
4
```

**Explanation**

The prime numbers less than `10` are:

```text
2, 3, 5, 7
```

Hence, the answer is `4`.

---

### Example 2

**Input**
```text
n = 0
```

**Output**
```text
0
```

---

### Example 3

**Input**
```text
n = 2
```

**Output**
```text
0
```

---

## Approach (Sieve of Eratosthenes)

Instead of checking every number individually for primality, we assume every number is prime initially and eliminate the multiples of each prime.

### Algorithm

1. If `n <= 2`, return `0`.
2. Create a boolean array `isPrime` of size `n`.
3. Mark every index from `2` to `n - 1` as `true`.
4. Traverse from `2` while `i * i < n`.
5. If `i` is prime, mark all multiples of `i` starting from `i * i` as `false`.
6. Count all remaining `true` values.

---

## Dry Run

### Input

```text
n = 10
```

### Initial State

| Number | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|--------:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| isPrime | T | T | T | T | T | T | T | T |

---

### Step 1 (`i = 2`)

Mark multiples of `2`:

```text
4, 6, 8
```

| Number | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|--------:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| isPrime | T | T | F | T | F | T | F | T |

---

### Step 2 (`i = 3`)

Mark multiples of `3`:

```text
9
```

| Number | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|--------:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| isPrime | T | T | F | T | F | T | F | F |

---

### Final Count

Remaining prime numbers:

```text
2, 3, 5, 7
```

Answer:

```text
4
```

---

## Why Start from `i × i`?

For `i = 5`, the multiples are:

```text
5 × 2 = 10
5 × 3 = 15
5 × 4 = 20
5 × 5 = 25
```

The numbers `10`, `15`, and `20` have already been marked by smaller primes (`2` and `3`).

Therefore, the first new multiple that needs to be marked is:

```text
5 × 5 = 25
```

This optimization avoids unnecessary work and improves efficiency.

---

## Complexity Analysis

- **Time Complexity:** `O(n log log n)`
- **Space Complexity:** `O(n)`

---

## Java Solution

```java
class Solution {
    public int countPrimes(int n) {
        if (n <= 2) {
            return 0;
        }

        boolean[] isPrime = new boolean[n];

        for (int i = 2; i < n; i++) {
            isPrime[i] = true;
        }

        for (int i = 2; i * i < n; i++) {
            if (isPrime[i]) {
                for (int j = i * i; j < n; j += i) {
                    isPrime[j] = false;
                }
            }
        }

        int count = 0;

        for (int i = 2; i < n; i++) {
            if (isPrime[i]) {
                count++;
            }
        }

        return count;
    }
}
```

---

## Key Takeaways

- A prime number has exactly two positive divisors: `1` and itself.
- The Sieve of Eratosthenes is the optimal algorithm for counting primes in a range.
- Start marking from `i × i` because smaller multiples are already processed.
- This approach is significantly faster than checking each number individually.
