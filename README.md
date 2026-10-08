# Student Academic Management Database (MySQL)

A SQL-only college database project for managing departments, students, faculty, courses, course offerings, enrollments, attendance, and marks. It includes the schema, a small sample dataset, and SQL answers for academic reporting questions 13–34.

![Entity relationship diagram](docs/student-academic-management-erd.jpeg)

## Project contents

```text
student-academic-management-mysql/
├── README.md
├── .gitignore
├── docs/
│   ├── README.md
│   └── student-academic-management-erd.jpeg
└── sql/
    ├── README.md
    ├── 01_create_database_and_schema.sql
    ├── 02_insert_sample_data.sql
    └── 03_queries_13_to_34_answers.sql
```

The scripts are ordered for a fresh database: create the database and tables, insert sample rows, then run selected queries. The query file contains some statements that change data or create objects; read [sql/README.md](sql/README.md) before running it.

## What the project demonstrates

- Relational modeling with primary keys, foreign keys, unique constraints, and check constraints.
- Department, student, faculty, course, offering, enrollment, attendance, and marks data.
- Joins, filtering, aggregation, grouping, and `HAVING` clauses.
- Attendance percentages and marks totals.
- Common table expressions and window functions for ranking.
- A performance view, transaction examples, and index examples.

## Database tables

| Table | Purpose | Key relationships |
| --- | --- | --- |
| `departments` | Department name, block, and head of department | Parent of students, faculty, and courses |
| `students` | Student identity, contact details, year, and department | References `departments` |
| `faculty` | Faculty identity, salary, designation, and department | References `departments` |
| `course` | Course name, credits, and owning department | References `departments` |
| `course_offering` | Course taught by faculty in a semester | References `course` and `faculty`; unique by course, faculty, and semester |
| `enrollment` | Student registration for a course and semester | References `students` and `course`; unique by student, course, and semester |
| `attendance` | Classes held and attended for a student-course pair | Composite primary key: `(s_id, c_id)` |
| `marks` | Midterm and external marks for a student-course pair | Composite primary key: `(s_id, c_id)` |

Each department may have many students, faculty members, and courses. Faculty and courses are connected many-to-many through `course_offering`; students and courses are connected many-to-many through `enrollment`.

The provided sample-data script inserts 20 rows into each of the eight tables. The sample names and `example.com` email addresses are illustrative.

## Requirements

- MySQL 8.0.16 or later. The query answers use CTEs and window functions, and the schema uses enforced `CHECK` constraints. MySQL documents CTEs and window functions in its [MySQL 8.0 manual](https://dev.mysql.com/doc/refman/8.0/en/mysql-nutshell.html) and notes that enforced `CHECK` constraints were introduced in [MySQL 8.0.16](https://dev.mysql.com/blog-archive/mysql-8-0-16-introducing-check-constraint/).
- MySQL Workbench or the MySQL command-line client is recommended.

## Setup

### MySQL Workbench

1. Open `sql/01_create_database_and_schema.sql` and execute it. It creates `college_db` if needed and creates the tables.
2. Open and execute `sql/02_insert_sample_data.sql` to load the sample rows.
3. Open `sql/03_queries_13_to_34_answers.sql`. Execute individual numbered sections as needed after reviewing the cautions in [sql/README.md](sql/README.md).

### MySQL command line

From the repository root in a Bash-compatible shell, run:

```bash
mysql -u root -p < sql/01_create_database_and_schema.sql
mysql -u root -p college_db < sql/02_insert_sample_data.sql
```

For the answer script, open a MySQL session on `college_db` and paste or source only the section you want to run. The full script includes a committed marks update and `CREATE INDEX` statements, so it is not a read-only query bundle.

## Query answer coverage

The answer file covers questions 13–34, including:

- Student, faculty, course, and department listings.
- Marks totals, average marks, attendance rates, and department counts.
- Course and department toppers, students with multiple enrollments, and courses with high enrollment.
- A semester result sheet and the `StudentPerformance` view.
- `COMMIT` and `ROLLBACK` examples, plus student-oriented indexes.

Some question text refers to values not present in the sample rows. For example, the sample student IDs are `S001`–`S020`, while question 17 filters for `S101`; and the sample course is `Database Management`, while question 18 filters for `Database Systems`. These queries need their filter values adjusted to return a sample result. More dataset-specific notes are in [sql/README.md](sql/README.md).

## Known modeling notes

- `attendance` and `marks` are keyed by `(s_id, c_id)` in the SQL schema. They do not reference `enrollment_id` or include a semester, so the schema cannot store separate attendance or marks for a student's repeat enrollment in the same course.
- The ER diagram depicts attendance and marks as one-to-one with enrollment. The SQL implementation instead links them directly to student and course using the composite keys above. See [docs/README.md](docs/README.md).
- The schema's `course` table is singular, and the student year column is named `year_`.
- This repository contains database scripts only; it does not include a web application, API, or user interface.

## Upload to GitHub

Create an empty repository on GitHub, then run these commands from this project directory. Replace the remote URL with your repository URL:

```bash
git init
git add .
git commit -m "Add student academic management database project"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repository>.git
git push -u origin main
```

## License

No license is included. Add a `LICENSE` file before publishing if you want to grant others specific rights to reuse or modify the project.
