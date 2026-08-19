![Lexicon Logo](https://lexicongruppen.se/media/wi5hphtd/lexicon-logo.svg)

# SQL Command Reference Guide

A reference for every MySQL command used in the exercises, with examples in **Workbench**, **Terminal**, **Java JDBC**, and **Lazysql**.

---

## Table of Contents

1. [CREATE DATABASE](#1-create-database)
2. [USE](#2-use)
3. [CREATE TABLE](#3-create-table)
4. [INSERT INTO](#4-insert-into)
5. [UPDATE SET](#5-update-set)
6. [DELETE FROM](#6-delete-from)
7. [SELECT](#7-select)
8. [WHERE](#8-where)
9. [LIKE](#9-like)
10. [INNER JOIN](#10-inner-join)
11. [LEFT JOIN](#11-left-join)
12. [COUNT](#12-count)
13. [GROUP BY](#13-group-by)

---

## 1. CREATE DATABASE

**What it does:** Creates a new database (schema) on the MySQL server.

**SQL:**
```sql
CREATE DATABASE school_management;
```

### Workbench
Open a query tab, paste the SQL, and click the **lightning bolt** icon (or press `Ctrl + Shift + Enter`).

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot -e "CREATE DATABASE school_management;"
```
Or enter the MySQL monitor first and run it there:
```bash
docker exec -it my-mysql mysql -uroot -proot
mysql> CREATE DATABASE school_management;
```

### Java JDBC
```java
String url = "jdbc:mysql://localhost:3306";
Connection conn = DriverManager.getConnection(url, "root", "root");
Statement stmt = conn.createStatement();
stmt.executeUpdate("CREATE DATABASE school_management");
```

### Lazysql
Open Lazysql (`lazysql`), navigate to the connection, then press `e` to open the SQL editor. Paste the command and execute.

---

## 2. USE

**What it does:** Selects a database to work with. All subsequent queries run against this database.

**SQL:**
```sql
USE school_management;
```

### Workbench
Run in query tab, or use the **schema selector** dropdown in the toolbar to pick `school_management`.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot -e "USE school_management;"
```
Or inside the MySQL monitor:
```sql
mysql> USE school_management;
```

### Java JDBC
```java
String url = "jdbc:mysql://localhost:3306/school_management";
Connection conn = DriverManager.getConnection(url, "root", "root");
```
Include the database name directly in the connection URL — no need for a separate `USE` statement.

### Lazysql
Select the database from the list when connecting, or switch to it from within the UI.

---

## 3. CREATE TABLE

**What it does:** Defines a new table with columns, data types, and constraints.

**SQL:**
```sql
CREATE TABLE courses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL,
    credits INT NOT NULL
);
```

| Keyword | Meaning |
|---------|---------|
| `INT` | Whole number |
| `VARCHAR(100)` | Text up to 100 characters |
| `PRIMARY KEY` | Uniquely identifies each row |
| `AUTO_INCREMENT` | MySQL assigns the next integer automatically |
| `NOT NULL` | Value is required |

### Workbench
Paste into a query tab and execute with the lightning bolt.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot school_management -e "
CREATE TABLE courses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL,
    credits INT NOT NULL
);"
```

### Java JDBC
```java
String url = "jdbc:mysql://localhost:3306/school_management";
Connection conn = DriverManager.getConnection(url, "root", "root");
Statement stmt = conn.createStatement();

String sql = "CREATE TABLE courses ("
           + "id INT AUTO_INCREMENT PRIMARY KEY, "
           + "course_name VARCHAR(100) NOT NULL, "
           + "credits INT NOT NULL"
           + ")";
stmt.executeUpdate(sql);
```

### Lazysql
Open the SQL editor (`e`) and paste the `CREATE TABLE` statement, then execute.

---

## 4. INSERT INTO

**What it does:** Adds new rows to a table.

**SQL — single row:**
```sql
INSERT INTO courses (course_name, credits)
VALUES ('Java Programming', 3);
```

**SQL — multiple rows:**
```sql
INSERT INTO courses (course_name, credits)
VALUES
  ('Java Programming', 3),
  ('SQL Basics', 8),
  ('Web Development', 5);
```

> Use single quotes (`'`) for string values.

### Workbench
Paste and run with the lightning bolt.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot school_management -e "
INSERT INTO courses (course_name, credits) VALUES
  ('Java Programming', 3),
  ('SQL Basics', 8),
  ('Web Development', 5);"
```

### Java JDBC
```java
String sql = "INSERT INTO courses (course_name, credits) VALUES (?, ?)";
PreparedStatement pstmt = conn.prepareStatement(sql);

pstmt.setString(1, "Java Programming");
pstmt.setInt(2, 3);
pstmt.executeUpdate();

pstmt.setString(1, "SQL Basics");
pstmt.setInt(2, 8);
pstmt.executeUpdate();

pstmt.setString(1, "Web Development");
pstmt.setInt(2, 5);
pstmt.executeUpdate();
```
> Always use `PreparedStatement` over `Statement` to prevent SQL injection.

### Lazysql
Open the SQL editor (`e`), paste the `INSERT` statement, and execute.

---

## 5. UPDATE SET

**What it does:** Modifies existing rows in a table.

**SQL:**
```sql
UPDATE courses
SET credits = 6
WHERE course_name = 'Java Programming';
```

> **Always include a `WHERE` clause.** Without it, every row in the table gets updated.

### Workbench
Paste and run. Verify with `SELECT * FROM courses;`.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot school_management -e "
UPDATE courses SET credits = 6 WHERE course_name = 'Java Programming';"
```

### Java JDBC
```java
String sql = "UPDATE courses SET credits = ? WHERE course_name = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setInt(1, 6);
pstmt.setString(2, "Java Programming");
int rowsAffected = pstmt.executeUpdate();
System.out.println("Rows updated: " + rowsAffected);
```

### Lazysql
Open the SQL editor (`e`), paste the `UPDATE` statement, and execute.

---

## 6. DELETE FROM

**What it does:** Removes rows from a table.

**SQL:**
```sql
DELETE FROM courses
WHERE id = 3;
```

> **Always include a `WHERE` clause.** Without it, all rows are deleted.

### Workbench
Paste and run. Verify with `SELECT * FROM courses;`.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot school_management -e "
DELETE FROM courses WHERE id = 3;"
```

### Java JDBC
```java
String sql = "DELETE FROM courses WHERE id = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setInt(1, 3);
int rowsAffected = pstmt.executeUpdate();
System.out.println("Rows deleted: " + rowsAffected);
```

### Lazysql
Open the SQL editor (`e`), paste the `DELETE` statement, and execute.

---

## 7. SELECT

**What it does:** Retrieves data from a table.

**SQL — all columns:**
```sql
SELECT * FROM courses;
```

**SQL — specific columns:**
```sql
SELECT course_name, credits FROM courses;
```

### Workbench
Paste and run. Results appear in the result grid below.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot school_management -e "SELECT * FROM courses;"
```

### Java JDBC
```java
String sql = "SELECT * FROM courses";
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(sql);

while (rs.next()) {
    int id = rs.getInt("id");
    String name = rs.getString("course_name");
    int credits = rs.getInt("credits");
    System.out.println(id + " | " + name + " | " + credits);
}
```

### Lazysql
Navigate to the table in the UI and press `l` to list rows, or use the SQL editor (`e`) with a `SELECT` query.

---

## 8. WHERE

**What it does:** Filters rows based on a condition.

**SQL:**
```sql
SELECT * FROM student
WHERE class_group = 'Java';
```

**Common operators:**
| Operator | Example |
|----------|---------|
| `=` | `WHERE credits = 3` |
| `!=` / `<>` | `WHERE credits != 3` |
| `>` / `<` | `WHERE credits > 3` |
| `>=` / `<=` | `WHERE credits >= 3` |
| `BETWEEN` | `WHERE credits BETWEEN 3 AND 6` |
| `IN` | `WHERE id IN (1, 2, 3)` |
| `IS NULL` | `WHERE credits IS NULL` |

### Workbench
Paste and run.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot school_management -e "
SELECT * FROM student WHERE class_group = 'Java';"
```

### Java JDBC
```java
String sql = "SELECT * FROM student WHERE class_group = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, "Java");
ResultSet rs = pstmt.executeQuery();

while (rs.next()) {
    System.out.println(rs.getString("name"));
}
```

### Lazysql
Use the SQL editor (`e`) with the `WHERE` clause.

---

## 9. LIKE

**What it does:** Pattern matching in a `WHERE` clause.

**SQL:**
```sql
SELECT * FROM student
WHERE name LIKE 'J%';
```

| Pattern | Meaning |
|---------|---------|
| `'J%'` | Starts with `J` |
| `'%son'` | Ends with `son` |
| `'%SQL%'` | Contains `SQL` |
| `'_'` | Matches any single character |

### Workbench
Paste and run.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot school_management -e "
SELECT * FROM student WHERE name LIKE 'J%';"
```

### Java JDBC
```java
String sql = "SELECT * FROM student WHERE name LIKE ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, "J%");
ResultSet rs = pstmt.executeQuery();

while (rs.next()) {
    System.out.println(rs.getString("name"));
}
```

### Lazysql
Use the SQL editor (`e`) with the `LIKE` clause.

---

## 10. INNER JOIN

**What it does:** Combines rows from two tables where the join condition matches. Rows without a match in both tables are excluded.

**SQL:**
```sql
SELECT student.name, attendance.status
FROM student
INNER JOIN attendance ON student.id = attendance.student_id;
```

**Visual:**
```
Table A        Table B
  ┌──┐           ┌──┐
  │  │    ┌────┐ │  │
  │  ├────┤    ├──┤  │
  │  │    │    │ │  │
  └──┘    └────┘ └──┘
       overlap only
```

### Workbench
Paste and run.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot school_management -e "
SELECT student.name, attendance.status
FROM student
INNER JOIN attendance ON student.id = attendance.student_id;"
```

### Java JDBC
```java
String sql = "SELECT student.name, attendance.status "
           + "FROM student "
           + "INNER JOIN attendance ON student.id = attendance.student_id";
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(sql);

while (rs.next()) {
    System.out.println(rs.getString("name") + " | " + rs.getString("status"));
}
```

### Lazysql
Use the SQL editor (`e`) with the `JOIN` clause.

---

## 11. LEFT JOIN

**What it does:** Combines rows from two tables. All rows from the left table are kept; if no match in the right table, `NULL` values are returned.

**SQL:**
```sql
SELECT student.name, attendance.status
FROM student
LEFT JOIN attendance ON student.id = attendance.student_id;
```

**Visual:**
```
Table A        Table B
  ┌──┐           ┌──┐
  │  ├────┐    ┌──┤  │
  │  ├────┤    ├──┤  │
  │  │    └────┘  │  │
  └──┘            └──┘
  all of A + matches
```

### Workbench
Paste and run.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot school_management -e "
SELECT student.name, attendance.status
FROM student
LEFT JOIN attendance ON student.id = attendance.student_id;"
```

### Java JDBC
```java
String sql = "SELECT student.name, attendance.status "
           + "FROM student "
           + "LEFT JOIN attendance ON student.id = attendance.student_id";
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(sql);

while (rs.next()) {
    System.out.println(rs.getString("name") + " | " + rs.getString("status"));
}
```

### Lazysql
Use the SQL editor (`e`) with the `LEFT JOIN` clause.

---

## 12. COUNT

**What it does:** Returns the number of rows.

**SQL — all rows:**
```sql
SELECT COUNT(*) FROM student;
```

**SQL — with condition:**
```sql
SELECT COUNT(*) FROM attendance
WHERE status = 'Present';
```

### Workbench
Paste and run.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot school_management -e "
SELECT COUNT(*) FROM student;"
```

### Java JDBC
```java
String sql = "SELECT COUNT(*) FROM student";
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(sql);

if (rs.next()) {
    int count = rs.getInt(1);
    System.out.println("Total students: " + count);
}
```

### Lazysql
Use the SQL editor (`e`) with the `COUNT()` function.

---

## 13. GROUP BY

**What it does:** Groups rows that share a value, usually combined with aggregate functions (`COUNT`, `SUM`, `AVG`, etc.).

**SQL — simple:**
```sql
SELECT status, COUNT(*) AS total
FROM attendance
GROUP BY status;
```

**SQL — with JOIN:**
```sql
SELECT student.name, COUNT(*) AS days_present
FROM student
INNER JOIN attendance ON student.id = attendance.student_id
WHERE attendance.status = 'Present'
GROUP BY student.name;
```

| Aggregate | Meaning |
|-----------|---------|
| `COUNT(*)` | Number of rows |
| `SUM(column)` | Total of numeric values |
| `AVG(column)` | Average of numeric values |
| `MIN(column)` | Smallest value |
| `MAX(column)` | Largest value |

### Workbench
Paste and run.

### Terminal
```bash
docker exec -it my-mysql mysql -uroot -proot school_management -e "
SELECT status, COUNT(*) AS total
FROM attendance
GROUP BY status;"
```

### Java JDBC
```java
String sql = "SELECT status, COUNT(*) AS total "
           + "FROM attendance GROUP BY status";
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(sql);

while (rs.next()) {
    System.out.println(rs.getString("status") + " | " + rs.getInt("total"));
}
```

### Lazysql
Use the SQL editor (`e`) with the `GROUP BY` clause.

---

## Quick Reference — Lazysql Keybindings

| Key | Action |
|-----|--------|
| `e` | Open SQL editor |
| `l` | List rows in selected table |
| `d` | Describe table structure |
| `q` | Go back / quit |
| `Ctrl + d` | Execute query in editor |

> Install Lazysql: `go install github.com/jorgerojas26/lazysql@latest`
> Then run: `lazysql`

---

## Quick Reference — Exercise ↔ Command Mapping

| Exercise | Command |
|----------|---------|
| 1.1 Create database | `CREATE DATABASE` |
| 1.2 Create table | `CREATE TABLE` |
| 2.1 Insert data | `INSERT INTO` |
| 2.2 Update data | `UPDATE SET` |
| 2.3 Delete data | `DELETE FROM` |
| 3.1 Select all | `SELECT *` |
| 3.2 Filter by group | `WHERE` |
| 3.3 Pattern match | `LIKE` |
| 4.1 Inner Join | `INNER JOIN` |
| 4.2 Left Join | `LEFT JOIN` |
| 5.1 Count | `COUNT` |
| 5.2 Group By | `GROUP BY` |
| 5.3 Group By + Join | `GROUP BY` + `JOIN` |
