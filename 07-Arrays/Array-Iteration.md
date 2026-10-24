# Array Iteration

## 1. Introduction

**Iteration** is the process of visiting every element of an array. Java offers two main styles: the **index-based for loop** (when you need positions) and the **enhanced for loop** (when you only need values).

---

## 2. Why This Topic Is Important

- Almost every array use involves iterating.
- Choosing the right loop style is a readability and correctness skill.
- Interviewers test iteration to print, sum, search, and transform arrays.
- Enhanced-for is used everywhere in modern Java.

---

## 3. Real-World Analogy

Iteration is like **reading every page of a book**:

- **Index for loop**: "Start at page 0, read page by page, and note the page number."
- **Enhanced for**: "Read each page in order." If you need to know the page number, you use the first style; otherwise the second is simpler.

---

## 4. Theory

### Index-Based Loop
```java
for (int i = 0; i < arr.length; i++) {
    arr[i];   // access by position
}
```
Use when: you need the **index** (for writing back, comparing neighbors, or printing positions).

### Enhanced For (for-each)
```java
for (int value : arr) {
    value;   // read-only access
}
```
Use when: you only **read** values in order.

### Key Differences
| Aspect | Index loop | Enhanced for |
|--------|-----------|--------------|
| Has index | ✅ | ❌ |
| Can modify elements | ✅ (`arr[i] = ...`) | ❌ |
| Can iterate backwards | ✅ | ❌ |
| Simpler/cleaner | ❌ | ✅ |

---

## 5. Syntax

```java
for (int i = 0; i < arr.length; i++) { ... }   // index
for (Type v : arr) { ... }                      // enhanced
```

---

## 6. Flow Explanation

```mermaid
flowchart LR
    A[Start i=0] --> B{i < length?}
    B -->|yes| C[process arr[i]]
    C --> D[i++]
    D --> B
    B -->|no| E[Done]
```

---

## 7. Memory Representation

Both loops use a local counter or iterator on the stack. No extra heap memory.

---

## 8. Simple Example — Print with Both Styles

```java
public class IterateDemo {
    public static void main(String[] args) {
        String[] fruits = {"Apple", "Mango", "Banana"};

        System.out.println("Index loop:");
        for (int i = 0; i < fruits.length; i++) {
            System.out.println(i + ": " + fruits[i]);
        }

        System.out.println("Enhanced for:");
        for (String f : fruits) {
            System.out.println("- " + f);
        }
    }
}
```

---

## 9. Intermediate Example — Sum, Max, Count

```java
public class Stats {
    public static void main(String[] args) {
        int[] nums = {12, 45, 3, 78, 22};

        int sum = 0;
        int max = nums[0];
        int evenCount = 0;

        for (int n : nums) {
            sum += n;
            if (n > max) max = n;
            if (n % 2 == 0) evenCount++;
        }

        System.out.println("Sum   : " + sum);
        System.out.println("Max   : " + max);
        System.out.println("Evens : " + evenCount);
    }
}
```

---

## 10. Advanced Example — Modify Elements (index loop needed)

```java
public class ModifyArray {
    public static void main(String[] args) {
        int[] scores = {60, 80, 40, 90};

        // double the scores using the index loop
        for (int i = 0; i < scores.length; i++) {
            scores[i] *= 2;
        }

        // print backwards
        System.out.print("Doubled (backwards): ");
        for (int i = scores.length - 1; i >= 0; i--) {
            System.out.print(scores[i] + " ");
        }
        System.out.println();
    }
}
```

---

## 11. Output

For the **Intermediate Example**:

```
Sum   : 160
Max   : 78
Evens : 2
```

For the **Advanced Example**:

```
Doubled (backwards): 180 80 160 120
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| `for (int n : arr) { n = 5; }` expecting modification | Enhanced-for can't modify elements; use index loop |
| Off-by-one in index loop | Use `i < arr.length`, not `<=` |
| Reusing `i` outside the loop | Loop variable is scoped inside |
| Modifying an array while enhanced-iterating | Not possible; use index loop or a copy |
| Forgetting `arr[i]` for sum | `for (int n : arr) sum += n;` |

---

## 13. Best Practices

- Use **enhanced-for** by default (read-only iteration).
- Switch to index loop only when you need positions or modifications.
- For repeated traversal, extract a helper method.
- Prefer `Arrays.stream(arr)` (Java 8) for functional pipelines (Module 16).

---

## 14. Interview Questions

**Q1. Difference between index loop and enhanced for?**
*Index loop gives the index and can modify elements; enhanced-for is read-only and cleaner.*

**Q2. Can you iterate an array backwards with enhanced for?**
*No — use an index loop from `length - 1` down to 0.*

**Q3. Which is faster?**
*Negligible difference; readability should decide.*

**Q4. Can enhanced-for be used on collections?**
*Yes — works on any Iterable and arrays.*

**Q5. What happens if the array is null?**
*`NullPointerException` — check for null first.*

---

## 15. Practice Exercises

1. Print all elements with their indices.
2. Compute sum, product, and count of negatives.
3. Print elements in reverse order.
4. Double every element and reprint.

---

## 16. Coding Challenges

1. **Easy:** Find the index of the largest element.
2. **Medium:** Check if an array is sorted in ascending order.
3. **Hard:** Find the pair of adjacent elements with the maximum sum.

---

## 17. Summary

- Index loop: positions, modification, reverse traversal.
- Enhanced-for: clean read-only iteration.
- Default to enhanced-for; use index when needed.
- Watch off-by-one and null arrays.

➡️ Continue to: [Array Methods](Array-Methods.md)