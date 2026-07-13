# Even Pair Formation

## Problem Statement

You are given an array of `N` integers. Determine whether it is possible to divide all the elements into pairs such that:

1. Every element belongs to exactly one pair.
2. The sum of the two numbers in each pair is **even**.

Return **"YES"** if such a pairing is possible; otherwise, return **"NO"**.

---

## Key Observation

The sum of two numbers is **even** only if:

- Even + Even = Even ✅
- Odd + Odd = Even ✅
- Even + Odd = Odd ❌

Therefore,

- All even numbers must be paired with other even numbers.
- All odd numbers must be paired with other odd numbers.

Hence,

- The number of even elements must be even.
- The number of odd elements must be even.

---

## Example 1

### Input

```text
N = 6
A = [2, 4, 1, 3, 6, 8]
```

### Output

```text
YES
```

### Explanation

Possible pairs:

```text
(2,4) → 6
(1,3) → 4
(6,8) → 14
```

Every pair has an even sum.

---

## Example 2

### Input

```text
N = 4
A = [1,2,3,4]
```

### Output

```text
NO
```

### Explanation

There is no way to pair every element such that every pair has an even sum.

---

## Example 3

### Input

```text
N = 8
A = [1,3,5,7,2,4,6,8]
```

### Output

```text
YES
```

### Explanation

Possible pairs:

```text
(1,3)
(5,7)
(2,4)
(6,8)
```

All sums are even.

---

# Approach 1: Adjacent Pair Checking (Naive Approach)

## Idea

Pair adjacent elements:

- (A[0], A[1])
- (A[2], A[3])
- (A[4], A[5])
- ...

If every adjacent pair has an even sum, return **YES**.

> **Note:** This approach is **incorrect** because the problem allows pairing any two elements, not just adjacent elements.

---

## Algorithm

1. If `N` is odd, return **NO**.
2. Traverse the array in pairs.
3. Compute the sum of every adjacent pair.
4. If any pair has an odd sum, return **NO**.
5. Otherwise return **YES**.

---

## Java Code

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();
        int[] arr = new int[n];

        for(int i = 0; i < n; i++){
            arr[i] = scan.nextInt();
        }

        if(n % 2 != 0){
            System.out.println("NO");
            return;
        }

        for(int i = 0; i < n; i += 2){

            int sum = arr[i] + arr[i + 1];

            if(sum % 2 != 0){
                System.out.println("NO");
                return;
            }
        }

        System.out.println("YES");
    }
}
```

---

## Dry Run

Input

```text
[2,4,1,3]
```

Pairs:

| Pair | Sum | Even? |
|------|-----|--------|
| (2,4) | 6 | Yes |
| (1,3) | 4 | Yes |

Output

```text
YES
```

---

### Limitation

Input

```text
[1,2,3,4]
```

Adjacent pairs:

```text
(1,2)
(3,4)
```

Both are odd.

Output:

```text
NO
```

However, this approach fails on many valid cases because it only checks adjacent elements instead of allowing any valid pairing.

---

## Complexity

- Time Complexity: **O(N)**
- Space Complexity: **O(1)**

---

# Approach 2: Count Even and Odd Elements (Optimal)

## Idea

Since

- Even + Even = Even
- Odd + Odd = Even

Count the number of even and odd elements.

A valid pairing is possible only when:

- Number of even elements is even.
- Number of odd elements is even.

---

## Algorithm

1. Count even numbers.
2. Count odd numbers.
3. If both counts are even, return **YES**.
4. Otherwise return **NO**.

---

## Java Code

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

        int even = 0;
        int odd = 0;

        for(int i = 0; i < n; i++){

            int num = scan.nextInt();

            if(num % 2 == 0)
                even++;
            else
                odd++;
        }

        if(even % 2 == 0 && odd % 2 == 0)
            System.out.println("YES");
        else
            System.out.println("NO");
    }
}
```

---

## Dry Run

Input

```text
[2,4,1,3,6,8]
```

Counting

| Element | Type |
|---------|------|
|2|Even|
|4|Even|
|1|Odd|
|3|Odd|
|6|Even|
|8|Even|

Total

```text
Even = 4
Odd = 2
```

Both counts are even.

Output

```text
YES
```

---

### Another Example

Input

```text
[1,2,3]
```

Count

```text
Even = 1
Odd = 2
```

Even count is odd.

Output

```text
NO
```

---

## Complexity Analysis

| Approach | Time | Space |
|----------|------|-------|
| Adjacent Pair Checking | O(N) | O(1) |
| Count Even & Odd (Optimal) | O(N) | O(1) |

---

# Key Takeaways

- An even sum is obtained only when both numbers have the same parity.
- Pairing adjacent elements is **not sufficient** because the problem allows any pairing.
- The optimal solution is to count even and odd elements.
- A valid pairing exists **if and only if** both the even count and odd count are even.
- The optimal approach runs in **O(N)** time with **O(1)** extra space.
