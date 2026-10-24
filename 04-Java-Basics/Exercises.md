# Exercises — Java Basics

> Practice the fundamentals of Module 04. Write each program, compile, run, and verify the output.

---

## 1. All About Variables

```java
public class AboutMe {
    public static void main(String[] args) {
        String name = "Your Name";
        int age = 25;
        double height = 5.9;
        char grade = 'A';
        boolean isStudent = true;

        System.out.println("Name   : " + name);
        System.out.println("Age    : " + age);
        System.out.println("Height : " + height);
        System.out.println("Grade  : " + grade);
        System.out.println("Student: " + isStudent);
    }
}
```

**Task:** Change the values to your own and run it.

---

## 2. Data Type Ranges

Print the min and max of each numeric type:

```java
public class Ranges {
    public static void main(String[] args) {
        System.out.println("byte   : " + Byte.MIN_VALUE + " to " + Byte.MAX_VALUE);
        System.out.println("short  : " + Short.MIN_VALUE + " to " + Short.MAX_VALUE);
        System.out.println("int    : " + Integer.MIN_VALUE + " to " + Integer.MAX_VALUE);
        System.out.println("long   : " + Long.MIN_VALUE + " to " + Long.MAX_VALUE);
    }
}
```

**Task:** Add `float` and `double` ranges using `Float.MIN_VALUE` / `Float.MAX_VALUE`.

---

## 3. Literal Practice

```java
public class Literals {
    public static void main(String[] args) {
        int decimal = 100;
        int hex = 0x64;
        int binary = 0b1100100;
        long big = 1_000_000_000L;
        float pi = 3.14159f;
        char letter = '\u0041';
        System.out.println("decimal=" + decimal + " hex=" + hex + " binary=" + binary);
        System.out.println("big=" + big + " pi=" + pi + " letter=" + letter);
    }
}
```

**Question:** Are `0x64` and `0b1100100` both equal to 100?

---

## 4. Casting

```java
public class Cast {
    public static void main(String[] args) {
        double d = 9.99;
        int i = (int) d;
        long l = 123456789L;
        int fromLong = (int) l;
        System.out.println("(int) 9.99      = " + i);
        System.out.println("(int) 123456789L = " + fromLong);
    }
}
```

**Task:** Predict the output before running. Did `fromLong` overflow?

---

## 5. Calculator with Operators

```java
import java.util.Scanner;

public class Calc {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter a: ");
        int a = sc.nextInt();
        System.out.print("Enter b: ");
        int b = sc.nextInt();

        System.out.println("Sum  : " + (a + b));
        System.out.println("Diff : " + (a - b));
        System.out.println("Prod : " + (a * b));
        System.out.println("Quo  : " + ((double) a / b));
        System.out.println("Rem  : " + (a % b));
        sc.close();
    }
}
```

**Task:** Run with `a=7, b=2`. What is `7 / 2.0`?

---

## 6. Age Checker

```java
import java.util.Scanner;

public class AgeCheck {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter your age: ");
        int age = sc.nextInt();
        boolean canVote = age >= 18;
        System.out.println("Can vote: " + canVote);
        sc.close();
    }
}
```

**Task:** Add a line that prints "Senior" if `age >= 60` using the ternary operator.

---

## 7. Short-Circuit Explorer

```java
public class ShortCircuit {
    public static void main(String[] args) {
        String s = null;
        if (s != null && s.length() > 2) {
            System.out.println("Has length");
        } else {
            System.out.println("No NPE thanks to &&");
        }
    }
}
```

**Task:** Change `&&` to `&` and observe the crash. Explain why.

---

## 8. Escape Artistry

```java
public class Escape {
    public static void main(String[] args) {
        System.out.println("A\tB\tC");
        System.out.println("1\t2\t3");
        System.out.println("He said \"Java is fun!\"");
        System.out.println("Path: C:\\Java\\bin");
    }
}
```

**Task:** Add a Unicode heart `\u2665` somewhere.

---

## 9. Self-Assessment Checklist

- [ ] I can declare variables of all 8 primitive types
- [ ] I know the ranges of byte, short, int, long
- [ ] I can write hex, binary, and long literals
- [ ] I understand widening vs narrowing casts
- [ ] I can use arithmetic, relational, logical, and ternary operators
- [ ] I can read user input with Scanner
- [ ] I know the three comment styles
- [ ] I never name variables with keywords

---

## 10. Challenge Projects

1. **Temperature Converter:** Read a Celsius value, print Fahrenheit.
2. **Bill Splitter:** Read total bill and number of people, print per-person share (2 decimals).
3. **Word Count:** Read a line and print how many characters it has (using `.length()`).
4. **Even/Odd Checker:** Read a number and print whether it's even or odd.

---

## 🎉 Next Module

You've mastered the basics. Move to **[Module 05 — Control Statements](../05-Control-Statements/README.md)** — where your programs start making decisions.