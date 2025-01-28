# Indexes-in-PostgreSQL
PostgreSQL supports various types of indexes to optimize queries and improve performance. Here are the some commonly used index types in PostgreSQL for different types of data structures.

- Sample Table

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary NUMERIC(10, 2),
    join_date DATE,
    skills TEXT[],
    performance_score INT,
    last_login TIMESTAMP
);
```

- Insert some data in the employees tables

```sql
INSERT INTO employees (name, department, salary, join_date, skills, performance_score, last_login) VALUES
('John Doe', 'IT', 75000.00, '2020-05-15', ARRAY['Python', 'SQL', 'Java'], 85, '2025-01-20 09:15:00'),
('Jane Smith', 'HR', 60000.00, '2018-08-01', ARRAY['Recruitment', 'Conflict Resolution'], 78, '2025-01-25 10:45:00'),
('Michael Brown', 'Finance', 90000.00, '2021-01-10', ARRAY['Accounting', 'Excel', 'Taxation'], 92, '2025-01-26 14:00:00'),
('Emily Davis', 'IT', 82000.00, '2019-03-22', ARRAY['Python', 'JavaScript'], 88, '2025-01-22 08:30:00'),
('Chris Wilson', 'Marketing', 65000.00, '2022-07-05', ARRAY['SEO', 'Content Writing'], 76, '2025-01-21 11:00:00'),
('Sophia Johnson', 'IT', 70000.00, '2023-09-12', ARRAY['C++', 'Cloud Computing'], 81, '2025-01-27 12:15:00'),
('David Lee', 'Finance', 88000.00, '2020-11-03', ARRAY['Financial Analysis', 'Budgeting'], 90, '2025-01-23 13:45:00'),
('Emma Walker', 'HR', 58000.00, '2017-06-15', ARRAY['Employee Training', 'Recruitment'], 72, '2025-01-19 15:30:00'),
('Liam Turner', 'IT', 78000.00, '2018-12-20', ARRAY['Python', 'Cybersecurity'], 89, '2025-01-18 09:50:00'),
('Olivia Hall', 'Marketing', 62000.00, '2022-03-10', ARRAY['Social Media', 'Public Relations'], 74, '2025-01-24 10:10:00');
```

### B-Tree Index
- For equality (=) and range queries (<, >, <=, >=).
- Default index type in PostgreSQL.
- Organizes data in a balanced tree structure for fast lookup.

```sql
CREATE INDEX idx_employee_name ON employees (name);

SELECT * FROM employees WHERE name = 'John Doe';
```

### Hash Index
- For exact matches only (e.g., =).
- Not suitable for range queries.
- Uses a hash table to store and lookup values.
  
```sql
CREATE INDEX idx_employee_id_hash ON employees USING HASH (employee_id);

SELECT * FROM employees WHERE employee_id = 101;
```

### GIN (Generalized Inverted Index)
- For full-text search, array columns, or JSONB fields.
- Stores a mapping of each value in an array/document to its corresponding row.

```sql
CREATE INDEX idx_skills_gin ON employees USING GIN (skills);

SELECT * FROM employees WHERE skills @> ARRAY['Python'];
```

### Partial Index
- When you only need to index specific rows (e.g., active employees).
- Applies a WHERE condition to index a subset of rows.
```sql
CREATE INDEX idx_active_employees ON employees (last_login) WHERE last_login IS NOT NULL;

SELECT * FROM employees WHERE last_login IS NOT NULL;
```

### Unique Index
- To enforce uniqueness of column values.
- Ensures no duplicate values exist in the indexed column.

```sql
CREATE UNIQUE INDEX idx_unique_name ON employees (name);

SELECT * FROM employees WHERE name = 'John Doe';
```

### Composite Index
- For queries involving multiple columns in the WHERE clause.
- Indexes combinations of multiple columns.

```sql
CREATE INDEX idx_name_department ON employees (name, department);

SELECT * FROM employees WHERE name = 'John Doe' AND department = 'IT';
```

### Concurrent Index
- To create an index without locking the table during index creation.
- Builds the index in parallel with read and write operations.

```sql
CREATE INDEX CONCURRENTLY idx_salary ON employees (salary);

SELECT * FROM employees WHERE salary > 50000;
```



