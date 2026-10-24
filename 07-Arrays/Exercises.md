# Exercises — Arrays

> Practice the array concepts from Module 07. Write, compile, and run each program.

---

## 1. Create & Print

```java
import java.util.Arrays;

public class A1 {
    public static void main(String[] args) {
        int[] marks = {85, 90, 78, 92, 88};
        System.out.println("Length: " + marks.length);
        System.out.println("All   : " + Arrays.toString(marks));
        System.out.println("First : " + marks[0]);
        System.out.println("Last  : " + marks[marks.length - 1]);
    }
}
```

**Task:** Create a `double[]` and a `String[]` the same way.

---

## 2. Iteration Styles

```java
public class A2 {
    public static void main(String[] args) {
        String[] fruits = {"Apple", "Mango", "Banana", "Orange"};

        for (int i = 0; i < fruits.length; i++) {
            System.out.println(i + " -> " + fruits[i]);
        }
        System.out.println();
        for (String f : fruits) {
            System.out.println("Next: " + f);
        }
    }
}
```

**Task:** Print fruits in reverse order using an index loop.

---

## 3. Sum & Average

```java
public class A3 {
    public static void main(String[] args) {
        int[] nums = {10, 20, 30, 40, 50};
        int sum = 0;
        for (int n : nums) sum += n;
        double avg = (double) sum / nums.length;
        System.out.println("Sum : " + sum);
        System.out.println("Avg : " + avg);
    }
}
```

**Task:** Also find the largest element.

---

## 4. Arrays Utility

```java
import java.util.Arrays;

public class A4 {
    public static void main(String[] args) {
        int[] a = {9, 1, 5, 3, 7};
        Arrays.sort(a);
        System.out.println("Sorted  : " + Arrays.toString(a));
        System.out.println("Index of 5: " + Arrays.binarySearch(a, 5));

        int[] bigger = Arrays.copyOf(a, 8);
        System.out.println("Resized : " + Arrays.toString(bigger));
    }
}
```

**Task:** Fill a new array with -1 and print it.

---

## 5. Two-Dimensional Grid

```java
public class A5 {
    public static void main(String[] args) {
        int[][] grid = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        for (int[] row : grid) {
            for (int v : row) {
                System.out.print(v + " ");
            }
            System.out.println();
        }
    }
}
```

**Task:** Print the sum of each row, then each column.

---

## 6. Algorithm Builder

Write the following methods in a `ArrayAlgo` class:

1. `static int max(int[] arr)`
2. `static int min(int[] arr)`
3. `static int sum(int[] arr)`
4. `static int countEven(int[] arr)`
5. `static void reverse(int[] arr)`

Test with `{5, 8, 2, 9, 4}` and print all results.

---

## 7. Self-Assessment Checklist

- [ ] I can create arrays with literals and `new`
- [ ] I know default values for each type
- [ ] I can iterate with index and enhanced-for
- [ ] I can use `Arrays.sort`, `binarySearch`, `copyOf`, `fill`
- [ ] I can work with 2D arrays
- [ ] I can implement search, max, reverse, frequency

---

## 8. Challenge Projects

1. **Movie Ratings:** Store 5 movie ratings, find the highest, lowest, and average.
2. **Matrix Tools:** Write transpose, main-diagonal sum, and border-print for a 3x3 matrix.
3. **Word Frequency:** Count how many times each letter appears in a sentence (case-insensitive).
4. **Deduplicator:** Remove duplicates from an int array (return a new array).

---

## 🎉 Next Module

Move to **[Module 08 — Strings](../08-Strings/README.md)** — Java's most-used class.