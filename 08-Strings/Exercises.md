# Exercises — Strings

> Practice the string concepts from Module 08. Write, compile, and run each program.

---

## 1. Pool vs new

```java
public class S1 {
    public static void main(String[] args) {
        String a = "Java";
        String b = "Java";
        String c = new String("Java");

        System.out.println("a == b      : " + (a == b));
        System.out.println("a == c      : " + (a == c));
        System.out.println("a.equals(c) : " + a.equals(c));
    }
}
```

**Task:** Predict the output, then run it.

---

## 2. Core Methods

```java
public class S2 {
    public static void main(String[] args) {
        String s = "  Java Programming  ";
        System.out.println("Trimmed : '" + s.trim() + "'");
        System.out.println("Upper   : " + s.trim().toUpperCase());
        System.out.println("Sub     : " + s.trim().substring(0, 4));
        System.out.println("Contains: " + s.contains("Programming"));
        System.out.println("IndexOf : " + s.indexOf("Programming"));
    }
}
```

**Task:** Extract just "Programming" using substring + indexOf.

---

## 3. Comparisons

```java
public class S3 {
    public static void main(String[] args) {
        String input = "yes";
        System.out.println(input.equals("yes"));
        System.out.println(input.equals("YES"));
        System.out.println(input.equalsIgnoreCase("YES"));
    }
}
```

**Task:** Add a `compareTo` example that shows ordering.

---

## 4. Split & Join

```java
public class S4 {
    public static void main(String[] args) {
        String sentence = "Learn Java step by step";
        String[] words = sentence.split(" ");
        System.out.println("Words: " + words.length);

        String joined = String.join("-", words);
        System.out.println("Joined: " + joined);
    }
}
```

**Task:** Split a CSV "id,name,city" and print each field.

---

## 5. StringBuilder

```java
public class S5 {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        for (int i = 1; i <= 5; i++) {
            sb.append(i).append(" ");
        }
        System.out.println("Built: " + sb);
        System.out.println("Reverse: " + sb.reverse());
    }
}
```

**Task:** Compare this speed with `String +=` for 50,000 iterations.

---

## 6. Formatting

```java
public class S6 {
    public static void main(String[] args) {
        String item = "Laptop";
        double price = 49999.5;
        int qty = 2;
        System.out.printf("%-10s %10.2f %3d %12.2f%n",
                item, price, qty, price * qty);
    }
}
```

**Task:** Add two more rows to make a mini table.

---

## 7. Conversions

```java
public class S7 {
    public static void main(String[] args) {
        String ageText = "25";
        int age = Integer.parseInt(ageText);
        System.out.println("Age in 5 years: " + (age + 5));

        String pi = "3.14159";
        System.out.println("Pi rounded: " + String.format("%.2f",
                Double.parseDouble(pi)));
    }
}
```

**Task:** Handle a bad input ("twenty") gracefully.

---

## 8. Self-Assessment Checklist

- [ ] I understand the String pool and immutability
- [ ] I use `.equals()` not `==` for content
- [ ] I know substring end-index is exclusive
- [ ] I can split/join/replace strings
- [ ] I use StringBuilder for loops
- [ ] I can format with printf
- [ ] I can parse strings to numbers safely

---

## 9. Challenge Projects

1. **Palindrome Checker:** Read a word, check if it reads the same forwards/backwards (case-insensitive).
2. **Initials Generator:** "Ajay Kumar Singh" → "A.K.S."
3. **Word Frequency:** Count occurrences of each word in a sentence (use arrays for now).
4. **CSV Validator:** Parse "name,age,city", validate age is a number, print a clean profile.

---

## 🎉 Next Module

Move to **[Module 09 — Object-Oriented Programming](../09-Object-Oriented-Programming/README.md)** — the heart of Java.