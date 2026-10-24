# Exercises — Methods

> Practice the method concepts from Module 06. Write, compile, and run each program.

---

## 1. Method Basics

```java
public class E1 {
    static void printWelcome() {
        System.out.println("Welcome to Methods!");
    }

    public static void main(String[] args) {
        printWelcome();
        printWelcome();
    }
}
```

**Task:** Call `printWelcome()` three times.

---

## 2. Parameters

```java
public class E2 {
    static void greet(String name, int times) {
        for (int i = 0; i < times; i++) {
            System.out.println("Hi " + name);
        }
    }

    public static void main(String[] args) {
        greet("Ajay", 3);
    }
}
```

**Task:** Add a `by` (farewell) method that says goodbye.

---

## 3. Return Values

```java
public class E3 {
    static double kmToMiles(double km) {
        return km * 0.621371;
    }

    public static void main(String[] args) {
        double miles = kmToMiles(10);
        System.out.println("10 km = " + miles + " miles");
    }
}
```

**Task:** Add `milesToKm` and verify round-tripping.

---

## 4. Pass-by-Value Proof

```java
public class E4 {
    static void change(int x) {
        x = 100;
    }

    public static void main(String[] args) {
        int a = 10;
        change(a);
        System.out.println("a after change: " + a);   // predict!
    }
}
```

**Question:** What prints? Why?

---

## 5. Overloading

```java
public class E5 {
    static int area(int side) {
        return side * side;               // square
    }

    static int area(int length, int width) {
        return length * width;            // rectangle
    }

    public static void main(String[] args) {
        System.out.println("Square 4x4     : " + area(4));
        System.out.println("Rectangle 4x6  : " + area(4, 6));
    }
}
```

**Task:** Add a `double area(double radius)` for a circle.

---

## 6. Recursion

```java
public class E6 {
    static int factorial(int n) {
        if (n <= 1) return 1;
        return n * factorial(n - 1);
    }

    public static void main(String[] args) {
        for (int i = 1; i <= 6; i++) {
            System.out.println(i + "! = " + factorial(i));
        }
    }
}
```

**Task:** Trace `factorial(4)` on paper, frame by frame.

---

## 7. Static Counter

```java
public class E7 {
    static int objects = 0;

    E7() {
        objects++;
    }

    public static void main(String[] args) {
        new E7();
        new E7();
        new E7();
        System.out.println("Objects created: " + objects);
    }
}
```

**Task:** Add a static method `getCount()`.

---

## 8. Varargs

```java
public class E8 {
    static int product(int... nums) {
        int result = 1;
        for (int n : nums) result *= n;
        return result;
    }

    public static void main(String[] args) {
        System.out.println("product()         = " + product());
        System.out.println("product(2,3,4)    = " + product(2, 3, 4));
    }
}
```

**Task:** Handle the empty case by returning 1 (already done). Now write `average(int... nums)`.

---

## 9. Self-Assessment Checklist

- [ ] I can declare and call a method with parameters
- [ ] I understand pass-by-value for primitives and references
- [ ] I can return values of different types
- [ ] I can overload methods correctly
- [ ] I can write a recursive method with a base case
- [ ] I know when to use `static`
- [ ] I can use varargs

---

## 10. Challenge Projects

1. **Number Utilities:** Build a `NumUtils` class with static methods: `isEven`, `isPrime`, `reverse`, `digitSum`.
2. **Shape Calculator:** Overloaded `area` for square, rectangle, circle, triangle.
3. **Recursive Explorer:** Write `fib`, `gcd`, and `power` recursively; print a table comparing them.
4. **Logger:** A static varargs logger that prints a level and any number of messages.

---

## 🎉 Next Module

Move to **[Module 07 — Arrays](../07-Arrays/README.md)** — store and process collections of data.