# Combinations

## Problem

Given two integers `n` and `k`, return all possible combinations of `k` numbers chosen from the range `[1, n]`.

The order of elements does **not** matter.

### Example

**Input:**

```text
n = 4
k = 2
```

**Output:**

```text
[[1,2], [1,3], [1,4], [2,3], [2,4], [3,4]]
```

---

## Approach

We use **Backtracking** to generate all possible combinations.

### Key Idea

* Start choosing numbers from `1`.
* Add a number to the current combination.
* Recursively choose the next number from `i + 1`.
* When the combination contains `k` numbers, add it to the result.
* Remove the last element to backtrack and try another possibility.

Using `i + 1` ensures that combinations such as `[2,1]` are not generated after `[1,2]`.

---

## Algorithm

1. Create an empty `result` list.
2. Create an empty `current` list.
3. Start backtracking from `1`.
4. If `current.size() == k`, add a copy of `current` to `result`.
5. Otherwise, iterate from `start` to `n`.
6. Add the current number.
7. Recursively call backtracking with `i + 1`.
8. Remove the last number.
9. Return `result`.

---

## Java Solution

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> current = new ArrayList<>();

        backtrack(1, n, k, current, result);

        return result;
    }

    private void backtrack(
        int start,
        int n,
        int k,
        List<Integer> current,
        List<List<Integer>> result
    ) {
        if (current.size() == k) {
            result.add(new ArrayList<>(current));
            return;
        }

        for (int i = start; i <= n; i++) {
            current.add(i);

            backtrack(i + 1, n, k, current, result);

            current.remove(current.size() - 1);
        }
    }
}
```

---

## Dry Run

For:

```text
n = 4
k = 2
```

The recursion generates:

```text
[]
├── [1]
│   ├── [1,2] ✓
│   ├── [1,3] ✓
│   └── [1,4] ✓
│
├── [2]
│   ├── [2,3] ✓
│   └── [2,4] ✓
│
├── [3]
│   └── [3,4] ✓
│
└── [4]
```

Final result:

```text
[[1,2], [1,3], [1,4], [2,3], [2,4], [3,4]]
```

---

## Why `i + 1`?

Suppose we have:

```text
[1,2]
```

We don't want to generate:

```text
[2,1]
```

because combinations don't care about order.

Therefore, after choosing `i`, we only consider numbers **greater than `i`**:

```java
backtrack(i + 1, ...)
```

This prevents duplicate combinations.

---

## Complexity

The number of combinations is:

[
C(n,k) = \frac{n!}{k!(n-k)!}
]

### Time Complexity

```text
O(C(n,k) × k)
```

Each combination contains `k` elements and needs to be copied into the result.

### Space Complexity

```text
O(k)
```

for the recursion/current combination, excluding the output list.

---

## Important Concepts

* **Backtracking**
* **Recursion**
* **Combinations**
* **Decision tree**
* **Choose → Explore → Undo**
* `current.add(i)`
* Recursive call
* `current.remove(...)`

### Backtracking Pattern

```text
Choose
   ↓
Explore
   ↓
Undo
```

In code:

```java
current.add(i);       // Choose

backtrack(i + 1, ...); // Explore

current.remove(...);  // Undo
```

---

## Combination vs Permutation

| Feature             | Combination  | Permutation  |
| ------------------- | ------------ | ------------ |
| Order matters       | No           | Yes          |
| `[1,2]` and `[2,1]` | Same         | Different    |
| Main idea           | Choose       | Arrange      |
| Common technique    | Backtracking | Backtracking |

### Remember

> **Combination → Order does NOT matter.**
> **Permutation → Order DOES matter.**
