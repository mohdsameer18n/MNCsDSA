# Geek's Training

## Problem Statement

Geek is attending a training program for **N days**. Each day, he can perform **one** of the following activities:

- 🏃 Running
- 🥊 Fighting
- 📚 Learning Practice

Each activity awards a certain number of merit points.

**Constraint:** Geek **cannot perform the same activity on two consecutive days**.

Given a 2D matrix `mat[][]`, where:

- `mat[i][0]` → Running points on day `i`
- `mat[i][1]` → Fighting points on day `i`
- `mat[i][2]` → Learning points on day `i`

Find the **maximum total merit points** Geek can earn.

---

## Example

### Input

```text
mat = [
 [1, 2, 5],
 [3, 1, 1],
 [3, 3, 3]
]
```

### Output

```text
11
```

### Explanation

| Day | Activity | Points |
|-----|----------|--------|
| 1 | Learning | 5 |
| 2 | Running | 3 |
| 3 | Fighting | 3 |

Total = **5 + 3 + 3 = 11**

---

## Approach (Dynamic Programming)

### State Definition

Let:

- `dp[i][0]` = Maximum points till day `i` if Geek performs **Running** on day `i`
- `dp[i][1]` = Maximum points till day `i` if Geek performs **Fighting** on day `i`
- `dp[i][2]` = Maximum points till day `i` if Geek performs **Learning** on day `i`

---

## DP Transition

If Geek performs Running today, yesterday he could have done either Fighting or Learning.

```text
dp[i][0] = mat[i][0] + max(dp[i-1][1], dp[i-1][2])
```

If Geek performs Fighting today:

```text
dp[i][1] = mat[i][1] + max(dp[i-1][0], dp[i-1][2])
```

If Geek performs Learning today:

```text
dp[i][2] = mat[i][2] + max(dp[i-1][0], dp[i-1][1])
```

---

## Base Case

For the first day:

```text
dp[0][0] = mat[0][0]
dp[0][1] = mat[0][1]
dp[0][2] = mat[0][2]
```

---

## Final Answer

```text
max(dp[n-1][0], dp[n-1][1], dp[n-1][2])
```

---

# Java Solution (DP)

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

        int[][] mat = new int[n][3];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 3; j++) {
                mat[i][j] = scan.nextInt();
            }
        }

        int[][] dp = new int[n][3];

        dp[0][0] = mat[0][0];
        dp[0][1] = mat[0][1];
        dp[0][2] = mat[0][2];

        for (int i = 1; i < n; i++) {

            dp[i][0] = mat[i][0] + Math.max(dp[i - 1][1], dp[i - 1][2]);

            dp[i][1] = mat[i][1] + Math.max(dp[i - 1][0], dp[i - 1][2]);

            dp[i][2] = mat[i][2] + Math.max(dp[i - 1][0], dp[i - 1][1]);
        }

        int ans = Math.max(
                    Math.max(dp[n - 1][0], dp[n - 1][1]),
                    dp[n - 1][2]);

        System.out.println(ans);
    }
}
```

---

# Space Optimized Solution

Instead of storing all `n` rows, only keep the previous day's results.

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int n = scan.nextInt();

        int[][] mat = new int[n][3];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 3; j++) {
                mat[i][j] = scan.nextInt();
            }
        }

        int run = mat[0][0];
        int fight = mat[0][1];
        int learn = mat[0][2];

        for (int i = 1; i < n; i++) {

            int newRun = mat[i][0] + Math.max(fight, learn);

            int newFight = mat[i][1] + Math.max(run, learn);

            int newLearn = mat[i][2] + Math.max(run, fight);

            run = newRun;
            fight = newFight;
            learn = newLearn;
        }

        System.out.println(Math.max(run, Math.max(fight, learn)));
    }
}
```

---

# Dry Run

### Input

```text
1 2 5
3 1 1
3 3 3
```

### DP Table

| Day | Running | Fighting | Learning |
|----|---------:|----------:|----------:|
| 0 | 1 | 2 | 5 |
| 1 | 8 | 6 | 3 |
| 2 | 9 | 11 | 11 |

Final Answer:

```text
max(9, 11, 11) = 11
```

---

# Complexity Analysis

## DP Table

- **Time Complexity:** `O(N)`
- **Space Complexity:** `O(N)`

---

## Space Optimized

- **Time Complexity:** `O(N)`
- **Space Complexity:** `O(1)`

---

# Key Takeaways

- Dynamic Programming problem.
- State depends only on the **previous day**.
- Cannot choose the **same activity on consecutive days**.
- Space can be optimized from **O(N)** to **O(1)**.
- Frequently asked in coding interviews and placement rounds.
- 
