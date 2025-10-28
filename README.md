# Task 6 — Subqueries and Nested Queries  
  


---

## Objective
Use **subqueries** in `SELECT`, `WHERE`, and `FROM` clauses to build advanced SQL logic and filtering.

---

##  Database Schema

###  Departments Table
| Column | Type | Description |
|--------|------|-------------|
| department_id | INT | Primary key |
| department_name | VARCHAR(50) | Name of the department |

###  Employees Table
| Column | Type | Description |
|--------|------|-------------|
| employee_id | INT | Primary key |
| name | VARCHAR(50) | Employee name |
| department_id | INT | Foreign key referencing Departments |
| salary | DECIMAL(10,2) | Employee salary |

---

##  Table Creation & Sample Data

CREATE TABLE Departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50)
);

CREATE TABLE Employees (
    employee_id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT,
    salary DECIMAL(10,2),
    FOREIGN KEY (department_id) REFERENCES Departments(department_id)
);

INSERT INTO Departments (department_id, department_name) VALUES
(10, 'IT'),
(20, 'HR'),
(30, 'Finance');

INSERT INTO Employees (employee_id, name, department_id, salary) VALUES
(1, 'Alice', 10, 60000),
(2, 'Bob', 20, 45000),
(3, 'Charlie', 10, 75000),
(4, 'David', 30, 50000),
(5, 'Eva', 20, 80000);

SELECT * FROM Departments;

SELECT * FROM Employees;

SELECT name, salary
FROM Employees
WHERE salary > (SELECT AVG(salary) FROM Employees);

SELECT 
    name,
    (SELECT department_name 
     FROM Departments d 
     WHERE d.department_id = e.department_id) AS department
FROM Employees e;

SELECT department_name
FROM Departments d
WHERE EXISTS (
    SELECT 1
    FROM Employees e
    WHERE e.department_id = d.department_id
);

SELECT 
    name,
    salary,
    (SELECT MAX(salary) FROM Employees) AS highest_salary
FROM Employees;

SELECT name, salary, department_id
FROM Employees e1
WHERE salary > (
    SELECT AVG(salary)
    FROM Employees e2
    WHERE e1.department_id = e2.department_id
);


