# Searching

## 1. Introduction

**Searching** finds an element in a collection. **Linear search** scans everything; **binary search** halves a sorted space. Binary search is a mandatory interview topic.

---

## 2. Why This Topic Is Important

- Binary search is O(log n) — the difference between seconds and hours.
- It's asked in almost every DSA interview.
- It generalizes to "find the boundary" style problems.
- Java ships `Arrays.binarySearch()` and `Collections.binarySearch()`.

---

## 3. Real-World Analogy

- **Linear search**: flipping through a phone book page by page from the start.
- **Binary search**: opening the book at the middle, comparing the name, and discarding half each time — like a dictionary lookup.

---

## 4. Theory

### Linear Search
| Aspect | Detail |
|--------|--------|
| Works on | Unsorted or sorted |
| Complexity | O(n) worst, O(1) best |
| Space | O(1) |

### Binary Search
| Aspect | Detail |
|--------|--------|
| Requires | Sorted data |
| Complexity | O(log n) |
| Space | O(1) iterative, O(log n) recursive |

### When to Use
| Situation | Choice |
|-----------|--------|
| Unsorted, small | Linear search |
| Sorted data | Binary search |
| Frequent lookups | Hash map (O(1)) |
| Finding boundaries (first/last) | Binary search variants |

---

## 5. Syntax

```java
// Built-in
int index = Arrays.binarySearch(arr, key);

// If key absent, returns -(insertionPoint) - 1
```

---

## 6. Flow Explanation (Binary Search)

```mermaid
flowchart TD
    A[lo, hi, mid] --> B{a[mid] == key?}
    B -->|yes| C[Found]
    B -->|key smaller| D[hi = mid - 1]
    B -->|key larger| E[lo = mid + 1]
    D --> F{lo <= hi?}
    E --> F
    F -->|yes| A
    F -->|no| G[Not found]
```

---

## 7. Memory Representation

Both searches use O(1) extra space beyond the input array.

---

## 8. Simple Example — Linear Search

```java
public class LinearSearch {
    static int search(int[] a, int key) {
        for (int i = 0; i < a.length; i++) {
            if (a[i] == key) return i;
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] a = {5, 2, 9, 1, 7};
        System.out.println("9 at index: " + search(a, 9));
        System.out.println("42 at index: " + search(a, 42));
    }
}
```

---

## 9. Intermediate Example — Binary Search

```java
public class BinarySearch {
    static int search(int[] a, int key) {
        int lo = 0, hi = a.length - 1;

        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;      // avoids overflow
            if (a[mid] == key) return mid;
            if (a[mid] < key) lo = mid + 1;
            else hi = mid - 1;
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] a = {10, 20, 30, 40, 50};
        System.out.println("30 at index: " + search(a, 30));
        System.out.println("45 at index: " + search(a, 45));
    }
}
```

---

## 10. Advanced Example — First Occurrence (Boundary)

```java
public class FirstOccurrence {
    // find the first index where key appears
    static int first(int[] a, int key) {
        int lo = 0, hi = a.length - 1, result = -1;

        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (a[mid] == key) {
                result = mid;
                hi = mid - 1;              // keep looking left
            } else if (a[mid] < key) {
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }
        return result;
    }

    public static void main(String[] args) {
        int[] a = {1, 2, 2, 2, 3, 4, 5};
        System.out.println("First 2 at index: " + first(a, 2));
    }
}
```

---

## 11. Output

For the **Simple Example**:

```
9 at index: 2
42 at index: -1
```

For the **Intermediate Example**:

```
30 at index: 2
45 at index: -1
```

For the **Advanced Example**:

```
First 2 at index: 1
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Binary search on unsorted data | Sort first (or use linear) |
| Infinite loop | Ensure lo/hi always move |
| `(lo + hi) / 2` overflow | Use `lo + (hi - lo) / 2` |
| `lo <= hi` vs `lo < hi` | Use `<=` for inclusive bounds |
| Forgetting `return -1` | Always have a not-found path |

---

## 13. Best Practices

- Binary search requires sorted input — sort first.
- Use `lo + (hi - lo) / 2` to avoid overflow.
- Prefer `Arrays.binarySearch()` in production.
- For first/last occurrences, remember the result and keep scanning.
- Test with empty, single, and duplicate arrays.

---

## 14. Interview Questions

**Q1. Linear vs binary search?**
*Linear O(n) any data; binary O(log n) sorted only.*

**Q2. What if the array has duplicates?**
*Binary search still finds one; use boundary variants for first/last.*

**Q3. Why `lo + (hi - lo) / 2`?**
*Avoids integer overflow for large arrays.*

**Q4. How do you search in a rotated sorted array?**
*Modified binary search — check which half is sorted.*

**Q5. What does Arrays.binarySearch return on miss?**
*`-(insertionPoint) - 1`.*

---

## 15. Practice Exercises

1. Implement linear search.
2. Implement iterative binary search.
3. Implement recursive binary search.
4. Find the last occurrence with a boundary variant.

---

## 16. Coding Challenges

1. **Easy:** Find a target in a sorted array.
2. **Medium:** Find the first and last index of a target.
3. **Hard:** Search in a rotated sorted array; find the square root via binary search.

---

## 17. Summary

- Linear: O(n), any data. Binary: O(log n), sorted only.
- Use `lo <= hi` and the safe mid formula.
- Boundary variants find first/last.
- Production: `Arrays.binarySearch()`.
- Duplicates and rotated arrays are interview favorites.

➡️ Continue to: [Time Complexity](Time-Complexity.md)