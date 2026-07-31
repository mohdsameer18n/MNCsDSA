# Gas Station

## Problem

There are `n` gas stations arranged in a circular route.

- `gas[i]` represents the amount of gas available at station `i`.
- `cost[i]` represents the gas required to travel from station `i` to `(i + 1) % n`.

Return the starting gas station's index if you can travel around the circuit exactly once in the clockwise direction. Otherwise, return `-1`.

---

# Approach: Greedy

## Intuition

To complete the circuit, the total amount of gas available must be at least the total cost required.

While traversing the stations, maintain the current fuel balance.

- If the balance becomes negative, it means we cannot reach the next station from the current starting point.
- Therefore, **none of the stations between the current start and the current station can be a valid starting point**.
- Reset the balance and choose the next station as the new starting point.

At the end:

- If the total gas is less than the total cost, return `-1`.
- Otherwise, return the identified starting station.

---

## Algorithm

1. Initialize:
   - `balance = 0`
   - `deficit = 0`
   - `start = 0`

2. Traverse all stations.

3. Update the current fuel balance:

   ```text
   balance += gas[i] - cost[i]
   ```

4. If the balance becomes negative:

   - Add the negative balance to `deficit`.
   - Reset `balance` to `0`.
   - Set the next station as the new starting point.

5. After the traversal:

   - If `balance + deficit >= 0`, return `start`.
   - Otherwise, return `-1`.

---

## Dry Run

### Example

```text
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]
```

| Station | Gas | Cost | Balance | Deficit | Start |
|---------:|----:|-----:|--------:|--------:|------:|
| 0 | 1 | 3 | -2 → Reset | -2 | 1 |
| 1 | 2 | 4 | -2 → Reset | -4 | 2 |
| 2 | 3 | 5 | -2 → Reset | -6 | 3 |
| 3 | 4 | 1 | 3 | -6 | 3 |
| 4 | 5 | 2 | 6 | -6 | 3 |

Final:

```text
balance + deficit = 6 + (-6) = 0
```

Since it is non-negative, the answer is:

```text
3
```

---

## Why Does This Greedy Approach Work?

Suppose we start from station `start`.

If at some station `i` the fuel balance becomes negative:

```text
balance < 0
```

This means we cannot even reach station `i + 1`.

Any station between `start` and `i` would have even less fuel available before reaching `i + 1`, so none of them can be a valid starting point.

Therefore, we safely skip all those stations and set:

```text
start = i + 1
```

This ensures each station is considered at most once, giving an optimal linear-time solution.

---

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - Each station is visited exactly once.

- **Space Complexity:** `O(1)`
  - Only a few variables are used.

---

## Java Solution

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {

        int balance = 0;
        int deficit = 0;
        int start = 0;

        for (int i = 0; i < gas.length; i++) {

            balance += gas[i] - cost[i];

            if (balance < 0) {
                deficit += balance;
                balance = 0;
                start = i + 1;
            }
        }

        return (balance + deficit >= 0) ? start : -1;
    }
}
```

---

## Key Takeaways

- The total gas must be **greater than or equal to** the total cost.
- A negative balance means the current starting station and every station before the failure point are invalid.
- Reset the start to the next station whenever the balance becomes negative.
- This greedy approach solves the problem in **O(n)** time with **O(1)** extra space.
