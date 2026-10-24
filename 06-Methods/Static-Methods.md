# Static Methods

## 1. Introduction

**Static methods** belong to the **class**, not to any object. You call them with the class name (`Math.max(...)`) without creating an instance. They're the workhorses of utility classes and the entry point `main`.

---

## 2. Why This Topic Is Important

- **Utility classes** (`Math`, `Collections`, `Arrays`) are almost entirely static.
- Interviewers test static vs instance differences thoroughly.
- You've been using static methods since Hello World (`main` is static).
- Understanding `static` is required before OOP (Module 09).

---

## 3. Real-World Analogy

A static method is like a **public library rule**: "You don't need your own copy of the rulebook — the rule applies to everyone at the library (class)." Instance methods are like **personal notebooks**: each member (object) has their own.

---

## 4. Theory

### Key Facts
| Fact | Detail |
|------|--------|
| Declaration | `static` keyword before return type |
| Calling | `ClassName.methodName(...)` or directly inside the same class |
| No `this` | Static methods have no `this` reference |
| No instance fields | Can't access non-static fields without an object |
| Shared | One copy for the entire class |
| `main` | `public static void main` — the JVM calls it without an object |

### Static vs Instance
| Aspect | Static | Instance |
|--------|--------|----------|
| Belongs to | Class | Object |
| Call | `Class.method()` | `object.method()` |
| Access instance fields | ❌ (no `this`) | ✅ |
| Memory | Method area (one copy) | Per object |

---

## 5. Syntax

```java
class Utility {
    static int add(int a, int b) {    // static method
        return a + b;
    }
}

// usage
Utility.add(2, 3);
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[Call Utility.add 2,3] --> B[JVM finds class Utility]
    B --> C[Method in method area]
    C --> D[Executes, returns 5]
    D --> E[No object needed]
```

---

## 7. Memory Representation

Static methods and static variables live in the **method area** (one copy for the class). Instance members live per object in the heap.

---

## 8. Simple Example

```java
public class StaticDemo {
    static void show() {
        System.out.println("Static method called without an object.");
    }

    public static void main(String[] args) {
        show();                    // direct call in same class
        StaticDemo.show();         // call with class name
    }
}
```

---

## 9. Intermediate Example — Math-style Utility Class

```java
public class MathUtils {
    static int square(int n) {
        return n * n;
    }

    static boolean isPrime(int n) {
        if (n < 2) return false;
        for (int i = 2; i * i <= n; i++) {
            if (n % i == 0) return false;
        }
        return true;
    }

    public static void main(String[] args) {
        System.out.println("square(9) = " + MathUtils.square(9));
        System.out.println("isPrime(17) = " + MathUtils.isPrime(17));
    }
}
```

---

## 10. Advanced Example — Static vs Instance Interaction

```java
public class StaticVsInstance {
    static int counter = 0;          // shared by all objects
    int instanceValue;               // per object

    StaticVsInstance(int v) {
        this.instanceValue = v;
        counter++;
    }

    static int getCounter() {        // static: only sees static state
        return counter;
    }

    int getValue() {                 // instance: sees object state
        return instanceValue;
    }

    public static void main(String[] args) {
        StaticVsInstance a = new StaticVsInstance(10);
        StaticVsInstance b = new StaticVsInstance(20);

        System.out.println("Counter (static, shared) = " + getCounter());
        System.out.println("a value = " + a.getValue());
        System.out.println("b value = " + b.getValue());
    }
}
```

---

## 11. Output

For the **Advanced Example**:

```
Counter (static, shared) = 2
a value = 10
b value = 20
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Calling a static method on `null` | Works, but confusing — call via class name |
| Accessing instance fields from a static method | Compile error — need an object reference |
| Calling an instance method from static context | Compile error — create an object first |
| Making everything static | OOP loses meaning; use instances for state |
| Thinking static methods are thread-safe | They can be if stateless; mutable static state is dangerous |

---

## 13. Best Practices

- Use static for **pure functions** (no state): `Math`, utilities.
- Keep static methods **stateless** to avoid threading issues.
- Call via the class name for clarity: `MathUtils.square(4)`.
- Reserve static for class-level behavior, not instance behavior.
- Avoid static mutable fields unless carefully synchronized.

---

## 14. Interview Questions

**Q1. What is a static method?**
*A method that belongs to the class itself, callable without creating an object.*

**Q2. Can a static method access instance variables?**
*No — there's no `this` reference; it can only access static members.*

**Q3. Why is main static?**
*So the JVM can call it before any object exists.*

**Q4. Can static methods be overloaded?**
*Yes — overloading works the same for static methods.*

**Q5. Can static methods be overridden?**
*They can be hidden (method hiding), but that's not true polymorphism/overriding.*

---

## 15. Practice Exercises

1. Create a `Calculator` class with static add/sub/mul/div.
2. Call a static method from `main` using both direct and class-name styles.
3. Build a static counter and confirm it's shared.
4. Try accessing an instance field from a static method — observe the error.

---

## 16. Coding Challenges

1. **Easy:** Static `isEven(int)` returning boolean.
2. **Medium:** Static `factorial(int)` (reuse recursion) in a `MathUtils` class.
3. **Hard:** Static `int countWords(String)` that returns word count, called without an object.

---

## 17. Summary

- Static = belongs to the **class**, not an object.
- Call via `ClassName.method()`; no `this` available.
- Cannot access instance members directly.
- Perfect for **utility/stateless** methods.
- `main` is static so the JVM can run it instantly.

➡️ Continue to: [Varargs](Varargs.md)