# MySQL Getting Started

## 1. Introduction

**MySQL** is the world's most popular open-source relational database. This topic walks you through installing MySQL, connecting to it, and running your first SQL — the concrete setup you will use with JDBC and Hibernate later.

## 2. Why This Topic Is Important

- Most Java backend tutorials and jobs assume MySQL.
- You need a running database to practice the entire module and later JDBC/JPA modules.
- Knowing how to connect and run queries is a daily skill.

## 3. Real-World Analogy

Installing MySQL is like **unboxing a server in your garage**: you install the engine, start it, open the console, and start giving it commands. From then on, any application can talk to it over the network.

## 4. Theory

### Options

| Option | Use case |
|--------|----------|
| MySQL Community Server | Full local server |
| MySQL Workbench | GUI for design and queries |
| Docker image | Fast, isolated: `docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql` |
| XAMPP/WAMP | Bundled stack for beginners |

### Key parts

- **Server** — a process listening on port 3306.
- **Client** — `mysql` CLI or Workbench connecting to it.
- **Database (schema)** — a namespace containing tables.
- **User** — account with grants (e.g., `root` or a dedicated app user).

### Connecting

```
mysql -u root -p
```

## 5. Syntax

```sql
CREATE DATABASE myapp CHARACTER SET utf8mb4;
USE myapp;
CREATE TABLE users (id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(100));
SHOW TABLES;
```

## 6. Flow Explanation

1. Install/start the MySQL server (service running).
2. Connect as root: `mysql -u root -p`.
3. Create an application database and user.
4. Create tables and run queries.
5. Later, JDBC connects with `jdbc:mysql://localhost:3306/myapp`.

## 7. Memory Representation

```
MySQL Server (:3306)
  └─ databases
       └─ myapp
            └─ users, orders, products
            └─ user accounts with grants
```

## 8. Simple Example

```bash
mysql -u root -p
```

```sql
CREATE DATABASE testdb;
USE testdb;
CREATE TABLE greet (id INT PRIMARY KEY AUTO_INCREMENT, msg VARCHAR(100));
INSERT INTO greet (msg) VALUES ('Hello MySQL');
SELECT * FROM greet;
```

## 9. Intermediate Example

```sql
CREATE USER 'app'@'localhost' IDENTIFIED BY 'secret';
GRANT ALL PRIVILEGES ON myapp.* TO 'app'@'localhost';
FLUSH PRIVILEGES;
SHOW DATABASES;
DESCRIBE users;
```

## 10. Advanced Example

```sql
-- Dump and restore
mysqldump -u root -p myapp > myapp.sql
mysql -u root -p myapp < myapp.sql
```

```bash
# Docker quick start
docker run --name mysql-demo -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=myapp -p 3306:3306 -d mysql:8
```

## 11. Output

```sql
SELECT * FROM greet;
```

```
+----+--------------+
| id | msg          |
+----+--------------+
|  1 | Hello MySQL  |
+----+--------------+
```

## 12. Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| Server not running | "Can't connect" errors | Start the MySQL service |
| Wrong port/host | Connection refused | Use `localhost:3306` |
| Using root in apps | Over-privileged | Create a scoped app user |
| Skipping `USE db;` | "No database selected" | Run `USE myapp;` |

## 13. Best Practices

- Create a dedicated user per application with minimal grants.
- Use `utf8mb4` charset for full Unicode support.
- Back up with `mysqldump` before schema changes.
- Never commit credentials; use environment variables/config.
- Use Docker for isolated, reproducible dev databases.

## 14. Interview Questions

**Q: What is the default MySQL port?**
3306.

**Q: How do you connect from Java?**
With JDBC: `DriverManager.getConnection("jdbc:mysql://localhost:3306/myapp", user, pass)`.

**Q: What is the difference between a database and a table?**
A database (schema) is a container holding many tables; tables hold the actual rows.

## 15. Practice Exercises

1. Install MySQL (native or Docker) and connect via CLI.
2. Create a `myapp` database and a `users` table.
3. Create an app user with grants and verify you can connect as them.

## 16. Coding Challenges

1. Create three related tables (users, orders, order_items) and run a join query.
2. Dump your database to a `.sql` file, drop the database, and restore it from the dump.

## 17. Summary

- MySQL is the go-to open-source relational database (port 3306).
- Connect via CLI/Workbench/Docker and run SQL directly.
- Create scoped users, use `utf8mb4`, back up regularly.
- This setup powers the next module, **Module 19 — JDBC**.