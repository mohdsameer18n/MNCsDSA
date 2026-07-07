# Time Conversion (12-Hour to 24-Hour Format)

## Problem Statement

Given a time in **12-hour AM/PM format**, convert it to **24-hour (military) format**.

### Notes

* `12:00:00AM` → `00:00:00`
* `12:00:00PM` → `12:00:00`

Return the converted time as a string.

---

## Example

### Input

```text
07:05:45PM
```

### Output

```text
19:05:45
```

### Explanation

* The input time is **7:05:45 PM**.
* Since it is **PM** and the hour is not `12`, add `12` to the hour.
* `7 + 12 = 19`
* Final answer: `19:05:45`

---

## Approach

1. Extract the last two characters (`AM` or `PM`).
2. Remove `AM/PM` from the string.
3. Split the remaining time into:

   * Hour
   * Minute
   * Second
4. Convert the hour:

   * If `AM` and hour is `12`, change hour to `00`.
   * If `PM` and hour is not `12`, add `12`.
5. Return the formatted time in `HH:mm:ss` format.

---

## Java Solution

```java
import java.util.*;

public class Main {

    public static String toConvertTime(String s) {

        String period = s.substring(s.length() - 2);
        String[] parts = s.substring(0, s.length() - 2).split(":");

        int hour = Integer.parseInt(parts[0]);
        String minutes = parts[1];
        String seconds = parts[2];

        if (period.equals("AM")) {
            if (hour == 12) {
                hour = 0;
            }
        } else {
            if (hour != 12) {
                hour += 12;
            }
        }

        return String.format("%02d:%s:%s", hour, minutes, seconds);
    }

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);
        String s = scan.nextLine();

        System.out.println(toConvertTime(s));
    }
}
```

---

## Dry Run

### Input

```text
07:05:45PM
```

### Execution

```
period = PM
hour = 7

PM && hour != 12
hour = 19

Output = 19:05:45
```

---

### Input

```text
12:00:00AM
```

### Execution

```
period = AM
hour = 12

hour = 0

Output = 00:00:00
```

---

### Input

```text
12:30:45PM
```

### Execution

```
period = PM
hour = 12

No change

Output = 12:30:45
```

---

## Important Points

### Why use `.equals()` instead of `==`?

In Java:

```java
period.equals("AM")
```

compares the **contents** of the string.

Using:

```java
period == "AM"
```

compares object references, which can produce incorrect results.

---

### Why is `AM/PM` removed from the output?

The required output is in **24-hour (military) format**.

Example:

```
07:05:45PM → 19:05:45
```

The suffix `AM` or `PM` is **not** included because the hour already indicates the time of day.

---

## Time Complexity

* **Time Complexity:** `O(1)`
* **Space Complexity:** `O(1)`

---

## Key Java Methods Used

| Method               | Purpose                                      |
| -------------------- | -------------------------------------------- |
| `substring()`        | Extract `AM`/`PM` and time portion           |
| `split(":")`         | Split the time into hour, minute, and second |
| `Integer.parseInt()` | Convert hour from `String` to `int`          |
| `String.format()`    | Format the hour with two digits (`%02d`)     |
