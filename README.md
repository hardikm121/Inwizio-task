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


# Inwizio-task3

## Retail Store Database Schema

### SQL Commands

#### 1. Create Database and Tables

```sql
CREATE DATABASE RetailStore;
USE RetailStore;

CREATE TABLE Customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(100),
    phone VARCHAR(20),
    address TEXT,
    join_date DATE
);


CREATE TABLE Products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2),
    stock_quantity INT
);


CREATE TABLE Orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10,2),
    order_status VARCHAR(20),
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);


CREATE TABLE OrderDetails (
    order_detail_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT,
    product_id INT,
    quantity INT,
    unit_price DECIMAL(10,2),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);


CREATE TABLE Payments (
    payment_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT,
    payment_date DATE,
    payment_amount DECIMAL(10,2),
    payment_method VARCHAR(20),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id)
);


INSERT INTO Customers (first_name, last_name, email, phone, address, join_date)
VALUES
('Rajesh', 'Kumar', 'rajesh.kumar@gmail.com', '9977553210', '123 MG Road, Indore', '2024-01-15'),
('Priya', 'Sharma', 'priya.sharma@gmail.com', '9777533220', '456 Bandra West, Mumbai', '2024-02-10'),
('Arjun', 'Verma', 'arjun.verma@gmail.com', '9886543130', '789 Koramangala, Bangalore', '2024-03-05');


INSERT INTO Products (product_name, category, price, stock_quantity)
VALUES
('Mobile Phone', 'Electronics', 15000.00, 30),
('LED TV', 'Electronics', 40000.00, 15),
('Dining Table', 'Furniture', 25000.00, 10),
('Office Chair', 'Furniture', 5000.00, 50),
('Notebook', 'Stationery', 50.00, 200);


INSERT INTO Orders (customer_id, order_date, total_amount, order_status)
VALUES
(1, '2023-03-10', 55000.00, 'Shipped'),
(2, '2023-03-12', 10000.00, 'Pending'),
(3, '2023-03-15', 25500.00, 'Shipped');


INSERT INTO OrderDetails (order_id, product_id, quantity, unit_price)
VALUES
(1, 1, 2, 15000.00),
(1, 2, 1, 40000.00),
(2, 4, 2, 5000.00),
(3, 3, 1, 25000.00),
(3, 5, 10, 50.00);


INSERT INTO Payments (order_id, payment_date, payment_amount, payment_method)
VALUES
(1, '2023-03-11', 55000.00, 'UPI'),
(3, '2023-03-16', 25500.00, 'Credit Card');
```

#### SQL Queries

1. **Find the Total Number of Orders for Each Customer**

```sql
SELECT c.customer_id, CONCAT(c.first_name, ' ', c.last_name) AS customer_name, COUNT(o.order_id) AS total_orders
FROM Customers c
LEFT JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, customer_name; 
```

2. **Find the Total Sales Amount for Each Product**

```sql
SELECT p.product_id, p.product_name, SUM(od.quantity * od.unit_price) AS total_revenue
FROM Products p
LEFT JOIN OrderDetails od ON p.product_id = od.product_id
GROUP BY p.product_id, p.product_name;
```

3. **Find the Most Expensive Product Sold**

```sql
SELECT p.product_name, od.unit_price AS max_price
FROM Products p
JOIN OrderDetails od ON p.product_id = od.product_id
WHERE od.unit_price = (
    SELECT MAX(unit_price)
    FROM OrderDetails
);
```

4. **Get the List of Customers Who Have Placed Orders in the Last 30 Days**

```sql
SELECT DISTINCT c.customer_id, CONCAT(c.first_name, ' ', c.last_name) AS customer_name
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
WHERE o.order_date >= CURDATE() - INTERVAL 30 DAY;
```

5. **Calculate the Total Amount Paid by Each Customer**

```sql
SELECT c.customer_id, CONCAT(c.first_name, ' ', c.last_name) AS customer_name, SUM(p.payment_amount) AS total_paid
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
JOIN Payments p ON o.order_id = p.order_id
GROUP BY c.customer_id, customer_name;
```

6. **Get the Number of Products Sold by Category**

```sql
SELECT p.category, SUM(od.quantity) AS total_sold
FROM Products p
JOIN OrderDetails od ON p.product_id = od.product_id
GROUP BY p.category;
```

7. **List All Orders That Are Pending**

```sql
SELECT o.order_id, o.order_date, o.total_amount, o.order_status
FROM Orders o
WHERE o.order_status = 'Pending';
```

8. **Find the Average Order Value**

```sql
SELECT AVG(total_amount) AS average_order_value
FROM Orders;
```

9. **List the Top 5 Customers Who Have Spent the Most Money**

```sql
SELECT c.customer_id, CONCAT(c.first_name, ' ', c.last_name) AS customer_name, SUM(o.total_amount) AS total_spent
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, customer_name
ORDER BY total_spent DESC
LIMIT 5;
```

10. **Find the Products That Have Never Been Sold**

```sql
SELECT p.product_id, p.product_name
FROM Products p
LEFT JOIN OrderDetails od ON p.product_id = od.product_id
WHERE od.order_id IS NULL;
```


# Inwizio-task4

## HR Management Database Schema

### SQL Commands

#### 1. Create Database and Tables

```sql
CREATE DATABASE HRmanagement;
USE HRmanagement;

CREATE TABLE Employees (
    EmployeeID INT AUTO_INCREMENT PRIMARY KEY,
    FirstName VARCHAR(100),
    LastName VARCHAR(100),
    Email VARCHAR(100),
    Phone VARCHAR(15),
    HireDate DATE,
    DepartmentID INT,
    ManagerID INT,
    Salary DECIMAL(10, 2),
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID),
    FOREIGN KEY (ManagerID) REFERENCES Employees(EmployeeID)
);


CREATE TABLE Departments (
    DepartmentID INT AUTO_INCREMENT PRIMARY KEY,
    DepartmentName VARCHAR(100),
    ManagerID INT
);



CREATE TABLE PerformanceReviews (
    ReviewID INT AUTO_INCREMENT PRIMARY KEY,
    EmployeeID INT,
    ReviewDate DATE,
    PerformanceScore VARCHAR(20),
    Comments TEXT,
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);


CREATE TABLE Payroll (
    PayrollID INT AUTO_INCREMENT PRIMARY KEY,
    EmployeeID INT,
    PaymentDate DATE,
    Amount DECIMAL(10, 2),
    PaymentMethod VARCHAR(20),
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);


ALTER TABLE Employees
ADD CONSTRAINT fk_manager
FOREIGN KEY (ManagerID) REFERENCES Employees(EmployeeID);



INSERT INTO Departments (DepartmentName, ManagerID) VALUES
('Human Resources', 101),
('Finance', 102),
('Information Technology', 103),
('Sales and Marketing', 104),
('Customer Support', 105),
('Operations', 106);




INSERT INTO Employees (FirstName, LastName, Email, Phone, HireDate, DepartmentID, ManagerID, Salary)
VALUES
('Rohan', 'Gupta', 'rohan.gupta@gmail.com', '9812345678', '2021-11-12', 3, NULL, 85000), 
('Sneha', 'Iyer', 'sneha.iyer@gmail.com', '9845678901', '2020-07-20', 4, NULL, 90000),  
('Arjun', 'Mehta', 'arjun.mehta@gmail.com', '9901234567', '2023-06-01', 5, NULL, 55000),
('Kavita', 'Patel', 'kavita.patel@gmail.com', '9876509876', '2019-01-01', 6, NULL, 80000); 

INSERT INTO Employees (FirstName, LastName, Email, Phone, HireDate, DepartmentID, ManagerID, Salary)
VALUES
('Amit', 'Sharma', 'amit.sharma@gmail.com', '9876543210', '2022-05-10', 1, 24, 75000), 
('Priya', 'Singh', 'priya.singh@gmail.com', '9823456789', '2023-02-15', 2, NULL, 60000), 
('Rahul', 'Verma', 'rahul.verma@gmail.com', '9876540987', '2022-12-15', 2, 24, 72000), 
('Anjali', 'Reddy', 'anjali.reddy@gmail.com', '9845676543', '2023-08-10', 4, 22, 58000), 
('Vikram', 'Deshmukh', 'vikram.deshmukh@gmail.com', '9812341122', '2020-03-05', 1, 21, 77000), 
('Pooja', 'Kulkarni', 'pooja.kulkarni@gmail.com', '9876578945', '2023-07-01', 3, 22, 64000); 



INSERT INTO PerformanceReviews (EmployeeID, ReviewDate, PerformanceScore, Comments)
VALUES
(37, '2023-01-10', 'Excellent', 'Consistently exceeds expectations.'),
(38, '2023-03-15', 'Good', 'Shows potential but needs improvement in teamwork.'),
(39, '2023-02-12', 'Average', 'Meets expectations but lacks initiative.'),
(40, '2023-04-25', 'Excellent', 'Outstanding performance in team projects.'),
(41, '2023-05-30', 'Good', 'Displays good work ethic but requires mentoring.'),
(42, '2022-12-20', 'Average', 'Handles responsibilities adequately.'),
(21, '2023-06-15', 'Poor', 'Needs to work on punctuality and consistency.'),
(22, '2023-08-20', 'Good', 'Proactive and helpful in resolving issues.'),
(23, '2023-03-05', 'Excellent', 'A reliable and hardworking team player.'),
(24, '2023-07-15', 'Good', 'Demonstrates good progress and commitment.');


INSERT INTO Payroll (EmployeeID, PaymentDate, Amount, PaymentMethod) 
VALUES 
(37, '2023-01-31', 75000, 'Bank Transfer'),
(41, '2023-01-31', 60000, 'Check'),
(38, '2023-01-31', 85000, 'Bank Transfer'),
(39, '2023-01-31', 90000, 'UPI'),
(21, '2023-01-31', 55000, 'Bank Transfer'),
(42, '2023-01-31', 80000, 'Check'),
(22, '2023-01-31', 72000, 'UPI'),
(40, '2023-01-31', 58000, 'Bank Transfer'),
(23, '2023-01-31', 77000, 'Check'),
(24, '2023-01-31', 64000, 'Bank Transfer');
```

#### SQL Queries

1. **Retrieve the names and contact details of employees hired after January 1, 2023**

```sql
SELECT FirstName, LastName, Email, Phone
FROM Employees
WHERE HireDate > '2023-01-01';
```

2. **Find the total payroll amount paid to each department**

```sql
SELECT e.DepartmentID, d.DepartmentName, SUM(p.Amount) AS TotalPayroll
FROM Payroll p
JOIN Employees e ON p.EmployeeID = e.EmployeeID
JOIN Departments d ON e.DepartmentID = d.DepartmentID
GROUP BY e.DepartmentID, d.DepartmentName;
```

3. **List all employees who have not been assigned a manager**

```sql
SELECT FirstName, LastName, Email, Phone
FROM Employees
WHERE ManagerID IS NULL;
```

4. ** Retrieve the highest salary in each department along with the employee’s name**

```sql
SELECT e.DepartmentID, d.DepartmentName, e.FirstName, e.LastName, e.Salary AS HighestSalary
FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID
WHERE e.Salary = (
    SELECT MAX(Salary)
    FROM Employees
    WHERE DepartmentID = e.DepartmentID
)
ORDER BY e.DepartmentID;
```

5. **Find the most recent performance review for each employee**

```sql
SELECT e.EmployeeID, e.FirstName, e.LastName, pr.ReviewDate, pr.PerformanceScore
FROM PerformanceReviews pr
JOIN Employees e ON pr.EmployeeID = e.EmployeeID
WHERE pr.ReviewDate = (
    SELECT MAX(ReviewDate)
    FROM PerformanceReviews
    WHERE EmployeeID = e.EmployeeID
);
```

6. **Count the number of employees in each department**

```sql
SELECT e.DepartmentID, d.DepartmentName, COUNT(e.EmployeeID) AS EmployeeCount
FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID
GROUP BY e.DepartmentID, d.DepartmentName;
```

7. **List all employees who have received a performance score of "Excellent"**

```sql
SELECT e.EmployeeID, e.FirstName, e.LastName, pr.PerformanceScore
FROM PerformanceReviews pr
JOIN Employees e ON pr.EmployeeID = e.EmployeeID
WHERE pr.PerformanceScore = 'Excellent';
```

8. **Identify the most frequently used payment method in payroll**

```sql
SELECT PaymentMethod, COUNT(*) AS Frequency
FROM Payroll
GROUP BY PaymentMethod
ORDER BY Frequency DESC
LIMIT 1;
```

9. **Retrieve the top 5 highest-paid employees along with their departments**

```sql
SELECT e.EmployeeID, e.FirstName, e.LastName, e.Salary, d.DepartmentName
FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID
ORDER BY e.Salary DESC
LIMIT 5;
```

10. **Show details of all employees who report directly to a specific manager**

```sql
SELECT e.EmployeeID, e.FirstName, e.LastName, e.Email, e.Phone, e.Salary
FROM Employees e
WHERE e.ManagerID = 101;
```

