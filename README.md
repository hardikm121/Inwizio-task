# Inwizio-task1

## University Database Schema

### SQL Commands

#### 1. Create Database and Tables

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
```

#### SQL Queries

1. **Find the Total Number of Students in Each Department**

```sql
SELECT d.department_name, COUNT(s.student_id) AS total_students
FROM Students s
JOIN Departments d ON s.department_id = d.department_id
GROUP BY d.department_name;
```

2. **List All Courses Taught by a Specific Professor**

```sql
SELECT c.course_name
FROM Courses c
JOIN Professors p ON c.professor_id = p.professor_id
WHERE LOWER(p.first_name) = 'anuj' AND LOWER(p.last_name) = 'pal';
```

3. **Find the Average Grade of Students in Each Course**

```sql
SELECT c.course_name, 
    AVG(
        CASE 
            WHEN e.grade = 'A' THEN 5.0
            WHEN e.grade = 'B' THEN 3.0
            WHEN e.grade = 'C' THEN 1.0
            WHEN e.grade = 'F' THEN 0.0
        END
    ) AS average_grade
FROM Enrollments e
JOIN Courses c ON e.course_id = c.course_id
GROUP BY c.course_name;
```

4. **List All Students Who Have Not Enrolled in Any Courses**

```sql
SELECT s.student_id, s.first_name, s.last_name
FROM Students s
LEFT JOIN Enrollments e ON s.student_id = e.student_id
WHERE e.course_id IS NULL;
```

5. **Find the Number of Courses Offered by Each Department**

```sql
SELECT d.department_name, COUNT(c.course_id) AS total_courses
FROM Courses c
JOIN Departments d ON c.department_id = d.department_id
GROUP BY d.department_name;
```

6. **List All Students Who Have Taken a Specific Course**

```sql
SELECT s.student_id, s.first_name, s.last_name
FROM Students s
JOIN Enrollments e ON s.student_id = e.student_id
JOIN Courses c ON e.course_id = c.course_id
WHERE c.course_name = 'DBMS';
```

7. **Find the Most Popular Course Based on Enrollment Numbers**

```sql
SELECT c.course_name, COUNT(e.enrollment_id) AS total_enrollments
FROM Courses c
JOIN Enrollments e ON c.course_id = e.course_id
GROUP BY c.course_name
ORDER BY total_enrollments DESC
LIMIT 1;
```

8. **Find the Average Number of Credits Per Student in a Department**

```sql
SELECT d.department_name, AVG(c.credits) AS avg_credits_per_student
FROM Students s
JOIN Enrollments e ON s.student_id = e.student_id
JOIN Courses c ON e.course_id = c.course_id
JOIN Departments d ON s.department_id = d.department_id
GROUP BY d.department_name;
```

9. **List All Professors Who Teach in More Than One Department**

```sql
SELECT p.professor_id, p.first_name, p.last_name, COUNT(DISTINCT c.department_id) AS departments_count
FROM Professors p
JOIN Courses c ON p.professor_id = c.professor_id
GROUP BY p.professor_id, p.first_name, p.last_name
HAVING COUNT(DISTINCT c.department_id) > 1;
```

10. **Get the Highest and Lowest Grade in a Specific Course**

```sql
SELECT c.course_name, MIN(e.grade) AS highest_grade, MAX(e.grade) AS lowest_grade
FROM Enrollments e
JOIN Courses c ON e.course_id = c.course_id
WHERE c.course_name = 'Data Structures'
GROUP BY c.course_name;
```


# Inwizio-task2

## Ride Application Database Schema

### SQL Commands

#### 1. Create Database and Tables

```sql
CREATE DATABASE Ola;
USE Ola;

CREATE TABLE Drivers(
    DriverID INT AUTO_INCREMENT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Phone VARCHAR(15),
    City VARCHAR(20),
    VehicleType VARCHAR(20),
	Rating FLOAT
);


CREATE TABLE Riders(
    RiderID INT AUTO_INCREMENT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
	Phone VARCHAR(15),
    City VARCHAR(20),
    JoinDate DATE
);


CREATE TABLE Rides(
	RideID INT AUTO_INCREMENT PRIMARY KEY,
	RiderID INT,
    DriverID INT,
    RideDate DATE,
    PickupLocation VARCHAR(200),
    DropLocation VARCHAR(200),
    Distance FLOAT,
    Fare FLOAT,
	RideStatus VARCHAR(50),
    FOREIGN KEY (RiderID) REFERENCES Riders(RiderID),
	FOREIGN KEY (DriverID) REFERENCES Drivers(DriverID)
);


CREATE TABLE Payments(
	PaymentID INT AUTO_INCREMENT PRIMARY KEY,
    RideID INT,
    PaymentMethod VARCHAR(50),
    Amount FLOAT,
    PaymentDate DATE,
FOREIGN KEY (RideID) REFERENCES Rides(RideID)
);


INSERT INTO Drivers (FirstName,LastName,Phone,City,VehicleType,Rating) VALUES
('Vikram', 'Singh', '9876543210', 'Mumbai', 'Sedan', 4.8),
('Ravi', 'Kumar', '8765432109', 'Delhi', 'SUV', 4.5),
('Amit', 'Sharma', '7654321098', 'Bangalore', 'Hatchback', 4.2),
('Rajesh', 'Gupta', '9875468791', 'Pune', 'Sedan', 4.9),
('Anuj', 'Yadav', '9985476152', 'Kolkata', 'SUV', 4.7);


INSERT INTO Riders (FirstName, LastName, Phone, City, JoinDate) VALUES
('Anushka', 'Soni', '9977501239', 'Mumbai', '2024-08-10'),
('Rahul', 'Pal', '9955409876', 'Delhi', '2024-05-15'),
('Sneha', 'Patil', '7954687125', 'Bangalore', '2024-04-20'),
('Punam', 'Pandey', '8754691241', 'Pune', '2024-07-25'),
('Aayush', 'Kumar', '9985476121', 'Kolkata', '2024-02-05');


INSERT INTO Rides (RiderID, DriverID, RideDate, PickupLocation, DropLocation, Distance, Fare, RideStatus) VALUES
(1, 1, '2024-05-01', 'Andheri', 'Bandra', 10.5, 150, 'Completed'),
(2, 2, '2024-05-02', 'Connaught Place', 'Dwarka', 15.2, 200, 'Completed'),
(3, 3, '2024-05-03', 'MG Road', 'Koramangala', 8.0, 100, 'Cancelled'),
(4, 4, '2024-05-04', 'Kothrud', 'Hinjewadi', 20.0, 250, 'Completed'),
(5, 5, '2024-05-05', 'Salt Lake', 'New Town', 12.0, 180, 'Ongoing');


INSERT INTO Payments (RideID, PaymentMethod, Amount, PaymentDate) VALUES
(1, 'Card', 150, '2024-05-01'),
(2, 'Cash', 200, '2024-05-02'),
(4, 'Wallet', 250, '2024-05-04');
```

#### SQL Queries

1. **Retrieve the names and contact details of all drivers with a rating of 4.5 or higher**

```sql
SELECT FirstName,LastName,Phone,Rating 
FROM Drivers
WHERE Rating >= 4.5; 
```

2. **Find the total number of rides completed by each driver**

```sql
SELECT d.FirstName, d.LastName, COUNT(r.RideID) AS TotalRides
FROM Drivers d
JOIN Rides r ON d.DriverID = r.DriverID
WHERE r.RideStatus = 'Completed'
GROUP BY d.DriverID, d.FirstName, d.LastName;
```

3. **List all riders who have never booked a ride**

```sql
SELECT r.FirstName, r.LastName
FROM Riders r
LEFT JOIN Rides rd ON r.RiderID = rd.RiderID
WHERE rd.RideID IS NULL;
```

4. **Calculate the total earnings of each driver from completed rides**

```sql
SELECT d.FirstName, d.LastName, SUM(r.Fare) AS TotalEarnings
FROM Drivers d
JOIN Rides r ON d.DriverID = r.DriverID
WHERE r.RideStatus = 'Completed'
GROUP BY d.DriverID, d.FirstName, d.LastName;
```

5. **Retrieve the most recent ride for each rider**

```sql
SELECT r.RiderID, r.FirstName, r.LastName, MAX(rd.RideDate) AS RecentRide
FROM Riders r
JOIN Rides rd ON r.RiderID = rd.RiderID
GROUP BY r.RiderID, r.FirstName, r.LastName;
```

6. **Count the number of rides taken in each city**

```sql
SELECT r.City, COUNT(rd.RideID) AS TotalRides
FROM Riders r
JOIN Rides rd ON r.RiderID = rd.RiderID
GROUP BY r.City;
```

7. **List all rides where the distance was greater than 20 km**

```sql
SELECT RideID, PickupLocation, DropLocation, Distance
FROM Rides
WHERE Distance > 20;
```

8. **Identify the most preferred payment method**

```sql
SELECT PaymentMethod, COUNT(PaymentID) AS TotalCount
FROM Payments
GROUP BY PaymentMethod
ORDER BY TotalCount DESC
LIMIT 1;
```

9. **Find the top 3 highest-earning drivers**

```sql
SELECT d.FirstName, d.LastName, SUM(r.Fare) AS TotalEarnings
FROM Drivers d
JOIN Rides r ON d.DriverID = r.DriverID
WHERE r.RideStatus = 'Completed'
GROUP BY d.DriverID, d.FirstName, d.LastName
ORDER BY TotalEarnings DESC
LIMIT 3;
```

10. **Retrieve details of all cancelled rides along with the rider's and driver's names**

```sql
SELECT rd.RideID, r.FirstName AS RiderFirstName, r.LastName AS RiderLastName,
d.FirstName AS DriverFirstName, d.LastName AS DriverLastName,
rd.PickupLocation, rd.DropLocation, rd.RideDate
FROM Rides rd
JOIN Riders r ON rd.RiderID = r.RiderID
JOIN Drivers d ON rd.DriverID = d.DriverID
WHERE rd.RideStatus = 'Cancelled';
```

