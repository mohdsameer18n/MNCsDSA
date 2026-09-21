# Password Validator

A simple Java program that validates whether a password meets basic security requirements.

## Password Requirements

A password is considered **valid** if it:

* Contains at least one lowercase character
* Contains at least one uppercase character
* Contains at least one digit
* Contains at least one special character
* Has more than 8 characters

## Java Code

```java
import java.util.*;

public class Main {

    public static boolean isValid(String password) {

        if (password == null || password.length() <= 8) {
            return false;
        }

        boolean lower = false;
        boolean upper = false;
        boolean digit = false;
        boolean spchar = false;

        for (char ch : password.toCharArray()) {

            if (Character.isLowerCase(ch)) {
                lower = true;
            }
            else if (Character.isUpperCase(ch)) {
                upper = true;
            }
            else if (Character.isDigit(ch)) {
                digit = true;
            }
            else {
                spchar = true;
            }
        }

        return lower && upper && digit && spchar;
    }

    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        String password = scan.next();

        if (isValid(password)) {
            System.out.println("Valid");
        }
        else {
            System.out.println("Not Valid");
        }

        scan.close();
    }
}
```

## Example 1

### Input

```text
Sameer@123
```

### Output

```text
Valid
```

### Explanation

| Requirement       | Result         |
| ----------------- | -------------- |
| Lowercase         | `ameer` ✅      |
| Uppercase         | `S` ✅          |
| Digit             | `123` ✅        |
| Special character | `@` ✅          |
| Length > 8        | 9 characters ✅ |

---

## Example 2

### Input

```text
sameer123
```

### Output

```text
Not Valid
```

The password does not contain:

* An uppercase character ❌
* A special character ❌

---

## How the Code Works

Four boolean variables are used:

```java
boolean lower = false;
boolean upper = false;
boolean digit = false;
boolean spchar = false;
```

The program checks every character:

```java
for (char ch : password.toCharArray()) {
```

### Lowercase

```java
if (Character.isLowerCase(ch)) {
    lower = true;
}
```

### Uppercase

```java
else if (Character.isUpperCase(ch)) {
    upper = true;
}
```

### Digit

```java
else if (Character.isDigit(ch)) {
    digit = true;
}
```

### Special character

```java
else {
    spchar = true;
}
```

Finally:

```java
return lower && upper && digit && spchar;
```

The password is valid only when **all four conditions are true**.

## Complexity

**Time Complexity:** `O(n)`

The password is traversed once.

**Space Complexity:** `O(1)`

Only a fixed number of boolean variables are used.

## Possible Improvements

For stronger password security, the application could:

* Increase minimum length to 12–16 characters
* Reject common passwords
* Check against compromised passwords
* Reject predictable patterns
* Rate-limit login attempts
* Store passwords using secure password-hashing algorithms such as Argon2id, bcrypt, or scrypt

## Technologies

* Java
* Java `Scanner`
* Java `Character` class
* Boolean-based validation
