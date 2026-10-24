# ResultSet

## 1. Introduction

A **`ResultSet`** is the cursor over the rows returned by a `SELECT`. You move forward with `next()`, read column values with typed getters, and iterate until the cursor passes the last row.

---

## 2. Why This Topic Is Important

- Reading results is the payoff of every query.
- Typed getters (`getInt`, `getString`) map DB types to Java.
- Interviewers test `next()`, getters, and null handling.
- ORMs map ResultSets to entities — understand the source.

---

## 3. Real-World Analogy

A ResultSet is like a **one-way aisle in a museum**: you walk forward from exhibit to exhibit (`next()`), and at each exhibit you look at specific plaques (columns) to learn details (`getString("name")`). You can't go backward, and you stop when the aisle ends.

---

## 4. Theory

### The Cursor Model
| Concept | Detail |
|---------|--------|
| Position | Starts before the first row |
| `next()` | Moves to the next row; false at end |
| `while (rs.next())` | Standard iteration |
| Column access | By name (`getString("name")`) or index (`getString(1)`) |

### Typed Getters
| Java type | Getter |
|-----------|--------|
| String | `getString` |
| int | `getInt` |
| long | `getLong` |
| double | `getDouble` |
| boolean | `getBoolean` |
| java.sql.Date | `getDate` |
| java.time.LocalDate (JDBC 4.2+) | `getObject(..., LocalDate.class)` |
| BigDecimal | `getBigDecimal` |

### Nulls
- SQL NULL → getter returns Java null (for objects) or 0/false (primitives).
- Check `rs.wasNull()` after reading to distinguish 0 vs NULL.

### ResultSet Types (advanced)
| Type | Behavior |
|------|----------|
| TYPE_FORWARD_ONLY | Default; forward only |
| TYPE_SCROLL_INSENSITIVE | Can `previous()`, `first()`, `last()` |
| CONCUR_READ_ONLY | Default; no updates through RS |

---

## 5. Syntax

```java
try (ResultSet rs = ps.executeQuery()) {
    while (rs.next()) {
        String name = rs.getString("name");
        int salary = rs.getInt("salary");
    }
}
```

---

## 6. Flow Explanation

```mermaid
flowchart TD
    A[executeQuery] --> B[Cursor before row 1]
    B --> C{next()?}
    C -->|Yes| D[Read columns]
    D --> C
    C -->|No| E[Done - close]
```

---

## 7. Memory Representation

The DB server holds the result; the ResultSet fetches rows as you advance (depends on fetch size).

---

## 8. Simple Example

```java
import java.sql.*;

public class ResultSetBasics {
    public static void main(String[] args) throws Exception {
        try (Connection conn = DriverManager.getConnection("jdbc:h2:mem:rs")) {

            try (Statement st = conn.createStatement()) {
                st.execute("CREATE TABLE emp (id INT, name VARCHAR(50), salary INT)");
                st.executeUpdate("INSERT INTO emp VALUES (1, 'Ajay', 90000)");
                st.executeUpdate("INSERT INTO emp VALUES (2, 'Priya', 70000)");
            }

            try (Statement st = conn.createStatement();
                 ResultSet rs = st.executeQuery("SELECT * FROM emp")) {

                System.out.println("Rows:");
                while (rs.next()) {
                    System.out.println(rs.getInt("id") + " "
                            + rs.getString("name") + " "
                            + rs.getInt("salary"));
                }
            }
        }
    }
}
```

---

## 9. Intermediate Example — By Index & wasNull

```java
import java.sql.*;

public class ResultSetIndex {
    public static void main(String[] args) throws Exception {
        try (Connection conn = DriverManager.getConnection("jdbc:h2:mem:rs2")) {

            try (Statement st = conn.createStatement()) {
                st.execute("CREATE TABLE emp (id INT, name VARCHAR(50), bonus INT)");
                st.executeUpdate("INSERT INTO emp VALUES (1, 'Ajay', 5000)");
                st.executeUpdate("INSERT INTO emp VALUES (2, 'Priya', NULL)");  // null bonus
            }

            try (Statement st = conn.createStatement();
                 ResultSet rs = st.executeQuery("SELECT id, name, bonus FROM emp")) {

                while (rs.next()) {
                    int id = rs.getInt(1);                 // by index
                    String name = rs.getString(2);
                    int bonus = rs.getInt(3);              // null -> 0
                    System.out.println(name + " bonus=" + bonus
                            + " (was null? " + rs.wasNull() + ")");
                }
            }
        }
    }
}
```

---

## 10. Advanced Example — Mapping Rows to Objects

```java
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

class Employee {
    final int id;
    final String name;
    final int salary;

    Employee(int id, String name, int salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    @Override
    public String toString() { return id + ": " + name + " (" + salary + ")"; }
}

public class MappingDemo {
    public static void main(String[] args) throws Exception {
        try (Connection conn = DriverManager.getConnection("jdbc:h2:mem:map")) {

            try (Statement st = conn.createStatement()) {
                st.execute("CREATE TABLE emp (id INT, name VARCHAR(50), salary INT)");
                st.executeUpdate("INSERT INTO emp VALUES (1, 'Ajay', 90000)");
                st.executeUpdate("INSERT INTO emp VALUES (2, 'Priya', 70000)");
            }

            List<Employee> employees = new ArrayList<>();
            try (Statement st = conn.createStatement();
                 ResultSet rs = st.executeQuery("SELECT * FROM emp")) {
                while (rs.next()) {
                    employees.add(new Employee(
                            rs.getInt("id"),
                            rs.getString("name"),
                            rs.getInt("salary")));
                }
            }

            employees.forEach(System.out::println);
        }
    }
}
```

---

## 11. Output

For the **Intermediate Example**:

```
Ajay bonus=5000 (was null? false)
Priya bonus=0 (was null? true)
```

For the **Advanced Example**:

```
1: Ajay (90000)
2: Priya (70000)
```

---

## 12. Common Mistakes

| Mistake | Correction |
|---------|------------|
| Reading columns before first next() | next() first |
| Using wrong getter for type | getString vs getInt etc. |
| Forgetting wasNull() | Distinguish 0 from NULL |
| Closing the Statement before reading | Statement close closes its ResultSet |
| Reading after loop ends | Guard with next() |

---

## 13. Best Practices

- Iterate with `while (rs.next())`.
- Access by column name for readability.
- Map rows to domain objects in a DAO.
- Use `wasNull()` when NULL matters.
- Close ResultSet (try-with-resources).

---

## 14. Interview Questions

**Q1. What is a ResultSet?**
*A cursor over query results; move with next() and read columns with getters.*

**Q2. What does next() return?**
*true if a row exists, false when past the last row.*

**Q3. How do you read a column?**
*By name or index: getString("name") / getString(1).*

**Q4. What does wasNull() do?**
*Reports whether the last getter read a SQL NULL.*

**Q5. Can you go backward in a ResultSet?**
*Only with TYPE_SCROLL_INSENSITIVE result sets.*

---

## 15. Practice Exercises

1. Iterate a ResultSet and print rows.
2. Read columns by index and name.
3. Detect NULL with wasNull.
4. Map query rows into Employee objects.

---

## 16. Coding Challenges

1. **Easy:** Print all users from a users table.
2. **Medium:** Aggregate salaries from a ResultSet (sum).
3. **Hard:** A mapper that converts any ResultSet into a List<Map<String,Object>>.

---

## 17. Summary

- ResultSet = cursor over rows.
- next() advances; getters read columns.
- wasNull() for NULL detection.
- Map rows to objects in DAOs.
- Close ResultSet and Statement.

➡️ Continue to: [Transactions](Transactions.md)