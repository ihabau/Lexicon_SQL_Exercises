![Lexicon Logo](https://lexicongruppen.se/media/wi5hphtd/lexicon-logo.svg)

# SQL & JDBC Exercises

Practice exercises for **G6220 – Java Development** at Lexicon.

---

## Getting Started

### Prerequisites

- MySQL server running (local or Docker)
- Database created and populated from the lecture (`student_db` with `student` and `attendance` tables)

### Docker Quick Setup

```bash
docker run -d --name my-mysql -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 mysql:latest
```

### How to Connect & Run SQL

Once your MySQL container is running, you can execute the SQL exercises using any of these methods:

#### Option A: MySQL Workbench (GUI)

1. Open MySQL Workbench and create a new connection:
   - **Hostname:** `127.0.0.1`
   - **Port:** `3306`
   - **Username:** `root`
   - **Password:** `root`
2. Open a query tab and paste your SQL statements, then execute.

#### Option B: MySQL Terminal (CLI)

```bash
docker exec -it my-mysql mysql -uroot -proot
```

This drops you into the MySQL monitor directly inside the container. Type your SQL and press `Enter` to run, or `;` to end a statement.

#### Option C: Java JDBC

Write your DDL/DML statements in Java using `JDBC`. Connect to `jdbc:mysql://localhost:3306` with user `root` and password `root`, then execute queries via `Statement` or `PreparedStatement`.

> **Tip:** For Exercise 1 (DDL), any of the three options works. If you plan to continue with the JDBC exercises later, Option C lets you stay in code the whole time.

### Resources

| Resource | Description |
|----------|-------------|
| [Teaching Materials (README)](../Lexicon_SQL/README.md) | Full walkthrough of both SQL and JDBC lectures with code examples and video timestamps |
| — [DDL: Creating Tables](../Lexicon_SQL/README.md#7-creating-a-database-and-table-ddl) | `CREATE DATABASE`, `CREATE TABLE`, data types, constraints |
| — [DML: Insert, Update, Delete](../Lexicon_SQL/README.md#8-inserting-data-dml) | `INSERT INTO`, `UPDATE ... SET`, `DELETE FROM` |
| — [DQL: SELECT & Filtering](../Lexicon_SQL/README.md#9-querying-data-dql) | `SELECT`, `WHERE`, `LIKE`, `BETWEEN`, `IN`, `ORDER BY` |
| — [JOINs](../Lexicon_SQL/README.md#18-joins--combining-tables) | `INNER JOIN`, `LEFT JOIN` |
| — [Aggregation](../Lexicon_SQL/README.md#17-aggregate-functions--group-by) | `COUNT`, `GROUP BY` |
| [SQL Presentation](./SQL_Presentation.md) | Full SQL theory: databases, DDL, DML, DQL, joins, constraints, ALTER TABLE |
| [SQL Exercises](./SQL_Exercises.md) | Exercise descriptions (original reference) |
| [SQL Command Guide](./GUIDE.md) | Every command with examples: Workbench, Terminal, Java JDBC, Lazysql |

---

## Exercise Checklist

### 1. DDL — Data Definition Language

| Ex          | Task | Lecture | Reference | Guide |
|-------------|------|---------|-----------|-------|
| [X] **1.1** | Create a database named `school_management` | [00:36:36](https://vimeo.com) | [Ch 7](../Lexicon_SQL/README.md#7-creating-a-database-and-table-ddl) | [CREATE DATABASE](./GUIDE.md#1-create-database) · [USE](./GUIDE.md#2-use) |
| [X] **1.2** | Create a `courses` table with columns: `id` (INT, PK, Auto Increment), `course_name` (VARCHAR(100), NOT NULL), `credits` (INT, NOT NULL) | [00:41:32](https://vimeo.com) | [Ch 7](../Lexicon_SQL/README.md#7-creating-a-database-and-table-ddl) · [Ch 14](../Lexicon_SQL/README.md#14-database-relationships--foreign-keys) | [CREATE TABLE](./GUIDE.md#3-create-table) |

### 2. DML — Data Manipulation Language

| Ex          | Task | Lecture | Reference | Guide |
|-------------|------|---------|-----------|-------|
| [X] **2.1** | Insert at least 3 courses into `courses` (e.g., 'Java Programming', 'SQL Basics', 'Web Development') | [00:47:59](https://vimeo.com) | [Ch 8](../Lexicon_SQL/README.md#8-inserting-data-dml) | [INSERT INTO](./GUIDE.md#4-insert-into) |
| [X] **2.2** | Update the credits for 'Java Programming' to a higher value | [01:20:47](https://vimeo.com) | [Ch 12](../Lexicon_SQL/README.md#12-updating-and-deleting-data-dml) | [UPDATE SET](./GUIDE.md#5-update-set) |
| [X] **2.3** | Delete a course from the table by its ID | [01:24:12](https://vimeo.com) | [Ch 12](../Lexicon_SQL/README.md#12-updating-and-deleting-data-dml) | [DELETE FROM](./GUIDE.md#6-delete-from) |

### 3. DQL — Data Query Language

| Ex          | Task | Lecture | Reference | Guide |
|-------------|------|---------|-----------|-------|
| [X] **3.1** | Select all columns from the `student` table | [00:51:00](https://vimeo.com) | [Ch 9](../Lexicon_SQL/README.md#9-querying-data-dql) | [SELECT](./GUIDE.md#7-select) |
| [X] **3.2** | Select students who belong to a specific `class_group` | [01:10:05](https://vimeo.com) | [Ch 10](../Lexicon_SQL/README.md#10-filtering-with-where-like-between-in) | [WHERE](./GUIDE.md#8-where) |
| [X] **3.3** | Find all students whose names start with the letter `'J'` | [01:13:23](https://vimeo.com) | [Ch 10](../Lexicon_SQL/README.md#10-filtering-with-where-like-between-in) | [LIKE](./GUIDE.md#9-like) |

### 4. Joins & Relationships

| Ex          | Task | Lecture | Reference | Guide |
|-------------|------|---------|-----------|-------|
| [X] **4.1** | Inner Join — display student names and their attendance status | [02:14:37](https://vimeo.com) | [Ch 18](../Lexicon_SQL/README.md#18-joins--combining-tables) | [INNER JOIN](./GUIDE.md#10-inner-join) |
| [X] **4.2** | Left Join — display all students and attendance, including students with no records | [02:19:30](https://vimeo.com) | [Ch 18](../Lexicon_SQL/README.md#18-joins--combining-tables) | [LEFT JOIN](./GUIDE.md#11-left-join) |

### 5. Aggregation & Grouping

| Ex          | Task | Lecture | Reference | Guide |
|-------------|------|---------|-----------|-------|
| [X] **5.1** | Count the total number of students in the `student` table | [02:08:37](https://vimeo.com) | [Ch 17](../Lexicon_SQL/README.md#17-aggregate-functions--group-by) | [COUNT](./GUIDE.md#12-count) |
| [X] **5.2** | Group By — count 'Present' vs 'Absent' records in `attendance` | [02:09:12](https://vimeo.com) | [Ch 17](../Lexicon_SQL/README.md#17-aggregate-functions--group-by) | [GROUP BY](./GUIDE.md#13-group-by) |
| [X] **5.3** | Group By with Join — calculate the number of days each student has been 'Present' | [02:09:12](https://vimeo.com) | [Ch 17](../Lexicon_SQL/README.md#17-aggregate-functions--group-by) · [Ch 18](../Lexicon_SQL/README.md#18-joins--combining-tables) | [GROUP BY](./GUIDE.md#13-group-by) + [INNER JOIN](./GUIDE.md#10-inner-join) |

---
## Project Structure

```
Lexicon_SQL_Exercises/
├── pom.xml
├── GUIDE.md                # Command reference (Workbench, Terminal, JDBC, Lazysql)
├── SQL_Exercises.md          # Exercise descriptions
├── SQL_Presentation.md       # SQL theory & concepts
└── src/
    └── main/java/se/lexicon/
        └── Main.java
```
