# Inwizio-task1 
# University Database schema

# University Database

This repository contains SQL scripts to create and manage a `University` database with tables for students, professors, departments, courses, and enrollments.

## SQL Commands

### 1. Create Database and Tables

```sql
CREATE DATABASE University;
USE University;

CREATE TABLE Students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(100),
    phone VARCHAR(20),
    date_of_birth DATE,
    enrollment_date DATE,
    department_id INT
);

CREATE TABLE Departments (
    department_id INT AUTO_INCREMENT PRIMARY KEY,
    department_name VARCHAR(100)
);

CREATE TABLE Professors (
    professor_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(100),
    phone VARCHAR(20)
);

ALTER TABLE Students
ADD FOREIGN KEY (department_id)
REFERENCES Departments(department_id);

CREATE TABLE Courses (
    course_id INT AUTO_INCREMENT PRIMARY KEY,
    course_name VARCHAR(100),
    department_id INT,
    professor_id INT,
    credits INT,
    FOREIGN KEY (department_id) REFERENCES Departments(department_id),
    FOREIGN KEY (professor_id) REFERENCES Professors(professor_id)
);

CREATE TABLE Enrollments (
    enrollment_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT,
    course_id INT,
    enrollment_date DATE,
    grade VARCHAR(5),
    FOREIGN KEY (student_id) REFERENCES Students(student_id),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);

INSERT INTO Departments (department_name) VALUES
('Computer Science'),
('Information Technology'),
('Electronics'),
('Mechanical');

INSERT INTO Professors (first_name, last_name, email, phone) VALUES
('Vivek', 'Sahu', 'vivek.sahu@university.edu', '9977625895'),
('Kanha', 'Soni', 'Kanha.soni@university.edu', '9876548215'),
('Ram', 'Yadav', 'ram.yadav@university.edu', '9988475612'),
('Anuj', 'Pal', 'anuj.pal@university.edu', '9985746854');

INSERT INTO Courses (course_name, department_id, professor_id, credits) VALUES
('DBMS', 1, 1, 4),
('Data Structures', 1, 1, 3),
('Maths', 2, 2, 3),
('Quantum Mechanics', 3, 3, 4),
('English', 4, 4, 3);

INSERT INTO Students (first_name, last_name, email, phone, date_of_birth, enrollment_date, department_id) VALUES
('Pratyush', 'Sahu', 'py.sahu@student.edu', '9876543210', '2000-05-15', '2023-09-01', 1),
('Anushka', 'Kanodia', 'anu@student.edu', '8765432109', '2001-07-20', '2023-09-01', 2),
('Ayush', 'Sharma', 'ayu.sh@student.edu', '7654321098', '1999-11-10', '2023-09-01', 3),
('Hardik', 'Malviya', 'hardikmalviya@student.edu', '8305258881', '2000-12-25', '2023-09-01', 4),
('Druve', 'Pal', 'druvepal@student.edu', '9956487564', '1998-03-30', '2023-09-01', 1);

INSERT INTO Enrollments (student_id, course_id, enrollment_date, grade) VALUES
(1, 1, '2023-09-10', 'A'),
(1, 2, '2023-09-11', 'B'),
(2, 3, '2023-09-12', 'A'),
(3, 4, '2023-09-13', 'C'),
(4, 5, '2023-09-14', 'B'),
(5, 1, '2023-09-15', 'A'),
(5, 2, '2023-09-16', 'A');


SELECT d.department_name, COUNT(s.student_id) AS total_students
FROM Students s
JOIN Departments d ON s.department_id = d.department_id
GROUP BY d.department_name;


