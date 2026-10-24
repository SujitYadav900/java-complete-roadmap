# Varargs

## 1. Introduction

**Varargs** (variable-length arguments) let a method accept **zero or more** arguments of the same type, passed as if they were individual values. Declared with three dots (`...`), varargs are implemented behind the scenes as an **array**.

---

## 2. Why This Topic Is Important

- `String.format`, `System.out.printf`, and logging frameworks use varargs heavily.
- It eliminates awkward array-building at call sites.
- Interviewers test varargs rules (position, array handling, overloads).
- It's a small feature with big API-design benefits.

---

## 3. Real-World Analogy

Varargs is like a **"grab bag" shopping bag**: "bring me any number of fruits" — one fruit, five fruits, or none. The bag (method) adapts to however many you hand over and holds them all.

---

## 4. Theory

### Declaration
```java
static void show(int... numbers) {   // varargs
    for (int n : numbers) {
        System.out.println(n);
    }
}
```

### Rules
| Rule | Detail |
|------|--------|
| Syntax | Type followed by `...` then a name |
| Placement | Must be the **last** parameter |
| Count | Only **one** varargs parameter per method |
| Calls | `show(1)`, `show(1,2,3)`, `show()` all valid |
| Inside | Treated as an array — `numbers.length` works |
| Conflict | Can't overload with an identical array version (`int[]` vs `int...` is a clash) |

### Zero or Many
```java
show();            // empty array length 0
show(5);           // one element
show(1, 2, 3, 4);  // four elements
```

---

## 5. Syntax

```java
static int sum(int... values) {
    int total = 0;
    for (int v : values) total += v;
    return total;
}
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[sum 1,2,3] --> B[Compiler builds int[]{1,2,3}]
    B --> C[Method receives array]
    C --> D[Loops and sums]
    D --> E[Returns 6]
```

---

## 7. Memory Representation

Varargs creates an **array in the heap** at each call. That's why calls with many arguments have slight overhead — usually negligible.

---

## 8. Simple Example

```java
public class VarargsDemo {
    static void printAll(String... items) {
        System.out.print("Got " + items.length + " item(s): ");
        for (String s : items) {
            System.out.print(s + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        printAll("Java", "Python");
        printAll("one");
        printAll();
    }
}
```

---

## 9. Intermediate Example — Sum of Any Count

```java
public class SumVarargs {
    static int sum(int... numbers) {
        int total = 0;
        for (int n : numbers) total += n;
        return total;
    }

    public static void main(String[] args) {
        System.out.println("sum()           = " + sum());
        System.out.println("sum(5)          = " + sum(5));
        System.out.println("sum(1,2,3)      = " + sum(1, 2, 3));
        System.out.println("sum(1..10)      = " + sum(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
    }
}
```

---

## 10. Advanced Example — Varargs with Normal Params

```java
public class VarargsAdvanced {
    static void log(String level, String... messages) {
        System.out.println("[" + level + "]");
        for (String m : messages) {
            System.out.println("  - " + m);
        }
    }

    static int max(int first, int... rest) {
        int best = first;
        for (int n : rest) {
            if (n > best) best = n;
        }
        return best;
    }

    public static void main(String[] args) {
        log("INFO", "Server started", "Port 8080", "Ready");
        System.out.println("max = " + max(3, 8, 1, 9, 4));
    }
}
```

---

## 11. Output

For the **Intermediate Example**:

```
sum()           = 0
sum(5)          = 5
sum(1,2,3)      = 6
sum(1..10)      = 55
```

For the **Advanced Example**:

```
[INFO]
  - Server started
  - Port 8080
  - Ready
max = 9
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Putting varargs first | Must be the **last** parameter |
| Declaring two varargs params | Only one allowed |
| Passing an array thinking it copies | It's used directly — changes inside affect it |
| Overloading with `int[]` version | Clash — can't have both `f(int...)` and `f(int[])` |
| Confusing `...` in calls | You call with normal values; `...` is only in the declaration |

---

## 13. Best Practices

- Place varargs last and keep at most one.
- Guard against ambiguity: prefer explicit overloads for 0 or 1 args if important.
- Use varargs only when the count is genuinely flexible.
- For hot loops, an explicit array may be faster (avoid per-call array creation).

---

## 14. Interview Questions

**Q1. What are varargs?**
*Variable-length arguments — a method can accept any number of values of one type, declared with `...`.*

**Q2. Are varargs arrays?**
*Yes — inside the method they are treated exactly like an array.*

**Q3. What are the placement rules?**
*The varargs parameter must be the last parameter, and only one is allowed.*

**Q4. Can a method overload with both `int...` and `int[]`?**
*No — they'd have the same signature after varargs desugaring, causing a clash.*

**Q5. What does a call with zero varargs do?**
*The method receives an empty array (length 0).*

---

## 15. Practice Exercises

1. Write `concat(String... parts)` that joins strings with a space.
2. Write `average(double... values)` handling the empty case.
3. Write `multiply(int... numbers)` returning 1 for no args.
4. Combine a normal param and varargs in one method.

---

## 16. Coding Challenges

1. **Easy:** `min(int... values)` returning the smallest.
2. **Medium:** `String join(String sep, String... words)` that inserts a separator.
3. **Hard:** Build a mini logging API: `log(LogLevel level, String format, Object... args)` using `String.format`.

---

## 17. Summary

- Varargs (`Type... name`) accept **zero or more** arguments.
- Implemented as an **array** internally.
- Must be the **last** parameter; only one per method.
- Avoid overloading with the equivalent array type.
- Great for flexible APIs like formatting and logging.

➡️ Continue to: [Exercises](Exercises.md)