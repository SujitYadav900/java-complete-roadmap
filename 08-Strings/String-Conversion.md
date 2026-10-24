# String Conversion

## 1. Introduction

**String conversion** means moving data between strings and other types: parsing **numbers from text** (`Integer.parseInt`), producing **text from numbers** (`String.valueOf`), and converting to/from **char arrays** and collections. Every real program does this constantly.

---

## 2. Why This Topic Is Important

- User input arrives as strings — you must **parse** them to numbers.
- Output APIs expect strings — you must **format** numbers into text.
- JSON, CSV, and file parsing all revolve around conversions.
- Interviewers test parse exceptions and the "parse vs valueOf" distinction.

---

## 3. Real-World Analogy

Think of conversions like **money exchange counters** at an airport. You hand in dollars (a number) and get rupees (a string) — or the reverse. Each counter (method) has a specific direction and currency; using the wrong one gives an error (exception).

---

## 4. Theory

### String → Numbers (Parsing)
| Method | Parses | Example |
|--------|--------|---------|
| `Integer.parseInt(s)` | int | `"42"` → 42 |
| `Long.parseLong(s)` | long | `"99"` → 99L |
| `Double.parseDouble(s)` | double | `"3.14"` → 3.14 |
| `Float.parseFloat(s)` | float | `"1.5"` → 1.5f |
| `Boolean.parseBoolean(s)` | boolean | `"true"` → true |

- Throws `NumberFormatException` on bad input.
- Auto-boxing variants exist but parse returns primitives.

### Number → String
| Method | Result |
|--------|--------|
| `String.valueOf(x)` | Handles any type, null-safe → "null" |
| `Integer.toString(x)` | int to string |
| `"" + x` | Concatenation trick (avoid for style) |

### Character ↔ Numbers
```java
int code = 'A';              // char -> int (65)
char ch = (char) 66;         // int -> char ('B')
int digit = Character.getNumericValue('7');  // 7
```

### String ↔ char[]
```java
char[] arr = s.toCharArray();        // to array
String back = new String(arr);       // array to string
```

---

## 5. Syntax

```java
int n = Integer.parseInt("100");
String s = String.valueOf(n);
char[] chars = s.toCharArray();
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[String "42"] -->|parseInt| B[int 42]
    B -->|valueOf| C[String "42"]
    D[char[]] -->|new String| E[String]
    E -->|toCharArray| D
```

---

## 7. Memory Representation

Parsing allocates a primitive (or boxed object); conversions create temporary Strings. No hidden costs beyond normal allocation.

---

## 8. Simple Example

```java
public class ConvertDemo {
    public static void main(String[] args) {
        String numText = "123";
        int num = Integer.parseInt(numText);
        System.out.println("Parsed + 1 = " + (num + 1));

        double d = Double.parseDouble("99.5");
        System.out.println("Parsed double = " + d);

        String back = String.valueOf(num);
        System.out.println("Back to string: " + back);
    }
}
```

---

## 9. Intermediate Example — Handling Bad Input

```java
public class SafeParse {
    static int safeParse(String s) {
        try {
            return Integer.parseInt(s);
        } catch (NumberFormatException e) {
            System.out.println("Invalid number: '" + s + "' -> using 0");
            return 0;
        }
    }

    public static void main(String[] args) {
        System.out.println(safeParse("42"));
        System.out.println(safeParse("abc"));
        System.out.println(safeParse("3.14"));   // not an int!
    }
}
```

---

## 10. Advanced Example — CSV Parsing Pipeline

```java
public class CsvPipeline {
    public static void main(String[] args) {
        String csv = "101,Ajay,25,55000.5";
        String[] fields = csv.split(",");

        int id = Integer.parseInt(fields[0].trim());
        String name = fields[1].trim();
        int age = Integer.parseInt(fields[2].trim());
        double salary = Double.parseDouble(fields[3].trim());

        System.out.println("ID    : " + id);
        System.out.println("Name  : " + name);
        System.out.println("Age   : " + age);
        System.out.println("Salary: " + String.format("%,.2f", salary));
    }
}
```

---

## 11. Output

For the **Intermediate Example**:

```
42
Invalid number: 'abc' -> using 0
Invalid number: '3.14' -> using 0
```

For the **Advanced Example**:

```
ID    : 101
Name  : Ajay
Age   : 25
Salary: 55,000.50
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| `Integer.parseInt("3.14")` | Throws — integers only; use `parseDouble` |
| Not handling `NumberFormatException` | Wrap in try/catch for user input |
| `Integer.parseInt(null)` | NPE — check null first |
| Parsing `" 42 "` with spaces | Fails; `trim()` first |
| Using `"" + x` in loops | Slow; use `String.valueOf` or StringBuilder |

---

## 13. Best Practices

- `trim()` input before parsing.
- Use `try/catch` around parsing untrusted input.
- Prefer `String.valueOf` for null-safe conversion.
- Use `Integer.parseInt` over `new Integer(s)` (deprecated).
- For locale-aware numbers, use `NumberFormat`.

---

## 14. Interview Questions

**Q1. Difference between parseInt and valueOf?**
*`Integer.parseInt` returns a primitive `int`; `Integer.valueOf` returns an `Integer` object.*

**Q2. What exception does parsing throw on invalid input?**
*`NumberFormatException` (a runtime exception).*

**Q3. How do you convert a char digit '7' to int 7?**
*`Character.getNumericValue('7')` or `'7' - '0'`.*

**Q4. How do you convert a String to a char array?**
*`s.toCharArray()`.*

**Q5. Is `String.valueOf(null)` safe?**
*Yes — it returns the string `"null"`.*

---

## 15. Practice Exercises

1. Parse your age from a String and add 1.
2. Convert an int to String three ways.
3. Parse a "name:age" line using split and conversion.
4. Build a safe parser that returns a default on error.

---

## 16. Coding Challenges

1. **Easy:** Sum two numbers typed as strings.
2. **Medium:** Parse a CSV line with 5 fields and validate each.
3. **Hard:** Convert an integer to a binary string (and back) using `Integer.toBinaryString` and `Integer.parseInt(bin, 2)`.

---

## 17. Summary

- Parse: `parseInt/parseDouble/parseBoolean`.
- Format: `String.valueOf`, `Integer.toString`.
- char↔number via `Character.getNumericValue` or casts.
- String↔char[] via `toCharArray` / `new String`.
- Always guard parsing with try/catch and trim input.

➡️ Continue to: [Exercises](Exercises.md)