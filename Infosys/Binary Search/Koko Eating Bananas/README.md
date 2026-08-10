# Koko Eating Bananas

## Problem Statement

Koko has several piles of bananas.

She can eat bananas at a constant speed of `k` bananas per hour.

For each pile:

* If the pile contains fewer than `k` bananas, Koko finishes it in one hour.
* Otherwise, she eats `k` bananas per hour from that pile.
* Koko can only eat from **one pile at a time**.

Given an integer array `piles` and an integer `h`, return the **minimum integer eating speed `k`** such that Koko can eat all the bananas within `h` hours.

### Example

```text
Input:
piles = [3,6,7,11]
h = 8

Output:
4
```

At speed `4`:

```text
3  → 1 hour
6  → 2 hours
7  → 2 hours
11 → 3 hours

Total = 8 hours
```

Therefore, the minimum speed is:

```text
4
```

---

# Approach

## Binary Search on Answer

We are not searching for an element in the array.

Instead, we search for the **minimum possible eating speed**.

The possible speeds are:

```text
1 ... max(piles)
```

### Why?

The minimum speed is `1`.

The maximum speed we need is `max(piles)` because at that speed Koko can finish the largest pile in one hour.

Therefore:

```text
left = 1
right = max(piles)
```

---

# How to Check a Speed

Suppose the current speed is:

```text
k = mid
```

For every pile, calculate the number of hours required:

```text
hours = ceil(pile / k)
```

Using integer arithmetic:

```java
(pile + k - 1) / k
```

Add the hours for all piles.

If:

```text
hours <= h
```

then the speed is valid.

We try a smaller speed.

If:

```text
hours > h
```

then the speed is too slow.

We need a larger speed.

---

# Java Code

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {

        int left = 1;
        int right = 0;

        // Find maximum pile
        for (int pile : piles) {
            right = Math.max(right, pile);
        }

        int answer = right;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            long hours = 0;

            // Calculate required hours
            for (int pile : piles) {
                hours += (pile + mid - 1) / mid;
            }

            if (hours <= h) {

                // Speed works, try smaller speed
                answer = mid;
                right = mid - 1;

            } else {

                // Speed is too slow
                left = mid + 1;
            }
        }

        return answer;
    }
}
```

---

# Dry Run

### Input

```text
piles = [3,6,7,11]
h = 8
```

Initial search range:

```text
left = 1
right = 11
```

---

### Step 1

```text
mid = 6
```

Calculate hours:

```text
3  → ceil(3/6)  = 1
6  → ceil(6/6)  = 1
7  → ceil(7/6)  = 2
11 → ceil(11/6) = 2
```

Total:

```text
1 + 1 + 2 + 2 = 6
```

Since:

```text
6 <= 8
```

Speed `6` works.

Try a smaller speed:

```text
right = 5
```

---

### Step 2

```text
left = 1
right = 5

mid = 3
```

Hours:

```text
3  → 1
6  → 2
7  → 3
11 → 4
```

Total:

```text
10
```

Since:

```text
10 > 8
```

Speed `3` is too slow.

Try a larger speed:

```text
left = 4
```

---

### Step 3

```text
left = 4
right = 5

mid = 4
```

Hours:

```text
3  → 1
6  → 2
7  → 2
11 → 3
```

Total:

```text
8
```

Since:

```text
8 <= 8
```

Speed `4` works.

Try smaller:

```text
right = 3
```

Now:

```text
left = 4
right = 3
```

Stop.

Answer:

```text
4
```

---

# Why `(pile + k - 1) / k`?

We need ceiling division.

For example:

```text
pile = 7
k = 3
```

Normal integer division:

```text
7 / 3 = 2
```

But Koko needs **3 hours**.

So we need:

```text
ceil(7 / 3) = 3
```

We calculate:

```java
(7 + 3 - 1) / 3
```

```text
9 / 3 = 3
```

Another example:

```text
11 / 4
```

```java
(11 + 4 - 1) / 4
= 14 / 4
= 3
```

Therefore:

```java
(pile + k - 1) / k
```

is a common way to perform **ceiling division using integers**.

---

# Why Binary Search Works

The number of hours is **monotonically decreasing** as the eating speed increases.

For example:

```text
Speed       Hours       Valid?
--------------------------------
1           27          ❌
2           15          ❌
3           10          ❌
4            8          ✅
5            8          ✅
6            6          ✅
7            6          ✅
...
```

The pattern is:

```text
❌ ❌ ❌ ❌ | ✅ ✅ ✅ ✅
            ↑
      Minimum valid speed
```

Once a speed works, every larger speed also works.

This monotonic property allows binary search.

---

# Algorithm

```text
1. Set left = 1.
2. Set right = max(piles).
3. Find mid.
4. Calculate total hours required at speed mid.
5. If hours <= h:
       Save mid as answer.
       Search left half.
6. Otherwise:
       Search right half.
7. Return answer.
```

---

# Complexity

Let:

```text
n = number of piles
M = maximum number of bananas in a pile
```

Binary search performs approximately:

```text
O(log M)
```

iterations.

For each iteration, we check all `n` piles.

Therefore:

```text
Time Complexity:  O(n log M)
Space Complexity: O(1)
```

---

# Pattern to Remember

```text
Koko Eating Bananas
        ↓
Binary Search on Answer
        ↓
Possible answers = 1 ... max(piles)
        ↓
Check if speed works
        ↓
hours <= h
   ↓           ↓
 YES           NO
 ↓              ↓
smaller       larger
```

## Key Template

```java
while (left <= right) {

    int mid = left + (right - left) / 2;

    if (isPossible(mid)) {
        answer = mid;
        right = mid - 1;
    } else {
        left = mid + 1;
    }
}
```

The most important idea is:

> **When the answer has a monotonic "possible / not possible" property, try Binary Search on the Answer.**
