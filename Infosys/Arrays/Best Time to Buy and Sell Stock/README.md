# 121. Best Time to Buy and Sell Stock

## Problem Statement

You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If no profit can be achieved, return `0`.

---

## Example 1

**Input:**

```text
prices = [7,1,5,3,6,4]
```

**Output:**

```text
5
```

**Explanation:**

```text
Buy on day 2 (price = 1)
Sell on day 5 (price = 6)

Profit = 6 - 1 = 5
```

---

## Example 2

**Input:**

```text
prices = [7,6,4,3,1]
```

**Output:**

```text
0
```

**Explanation:**

```text
Stock prices keep decreasing.
No profit can be made.
```

---

## Constraints

```text
1 <= prices.length <= 10^5
0 <= prices[i] <= 10^4
```

---

## Approach

* Keep track of the minimum stock price seen so far.
* For every day, calculate the profit if we sell on that day.
* Update the maximum profit whenever a larger profit is found.
* Return the maximum profit.

---

## Dry Run

Consider:

```text
prices = [7,1,5,3,6,4]
```

Initially:

```text
minPrice = ∞
maxProfit = 0
```

| Day | Price | minPrice | Current Profit | maxProfit |
| --- | ----- | -------- | -------------- | --------- |
| 1   | 7     | 7        | 0              | 0         |
| 2   | 1     | 1        | 0              | 0         |
| 3   | 5     | 1        | 4              | 4         |
| 4   | 3     | 1        | 2              | 4         |
| 5   | 6     | 1        | 5              | 5         |
| 6   | 4     | 1        | 3              | 5         |

### Visualization

```text
Day:    1  2  3  4  5  6
Price:  7  1  5  3  6  4
            ↑        ↑
          Buy      Sell
```

Buy at:

```text
Price = 1
```

Sell at:

```text
Price = 6
```

Profit:

```text
6 - 1 = 5
```

Answer:

```text
Maximum Profit = 5
```

---

## Java Solution

```java
class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;

        for (int price : prices) {

            if (price < minPrice) {
                minPrice = price;
            } else {
                maxProfit = Math.max(maxProfit, price - minPrice);
            }
        }

        return maxProfit;
    }
}
```

---

## Complexity Analysis

### Time Complexity

```text
O(n)
```

We traverse the array only once.

### Space Complexity

```text
O(1)
```

Only two variables are used.

---

## Key Idea

At every step:

* Track the lowest buying price.
* Calculate potential profit.
* Update the maximum profit.
* Continue until the end of the array.

This allows us to solve the problem in a single pass.
