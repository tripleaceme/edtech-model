# EdTech Platform Database Project

This repository contains SQL scripts to create and manage a database for an EdTech platform, meeting the requirements outlined in the case study.

## Project Scope

The EdTech platform requires a database to manage the following:

1. **Courses**: Information about available courses, including name, instructor, and duration.
2. **Students**: Details of registered students, such as names, email addresses, and registration dates.
3. **Enrollments**: Tracking of which students are enrolled in which courses, with enrollment dates and progress.

## Features Implemented

### Entities

- **Courses**:
  - Fields: `CourseID`, `Name`, `Category`, `InstructorID`, `Duration`.
  - Originally included `Description`, which was removed per requirements.

- **Students**:
  - Fields: `StudentID`, `FullName`, `Email`, `RegistrationDate`.

- **Enrollments**:
  - Fields: `EnrollmentID`, `CourseID`, `StudentID`, `EnrollmentDate`, `Progress`.

### Operations

- **Database creation**: `EdTechDB`
- **Tables**: `Courses`, `Students`, `Enrollments`
- **Data population**: Sample data for testing purposes
- **DDL Operations**:
  - Adding a `Category` column to classify courses.
  - Dropping the `Description` column from `Courses`.
- **DML Operations**:
  - Updating student progress in `Enrollments`.
  - Retrieving courses a specific student is enrolled in.
  - Deleting a student and their associated enrollments.

---

## Scripts Overview

### Database Setup

```sql
CREATE DATABASE EdTechDB;
USE EdTechDB;
```

### Tables and Data

#### Students Table

```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY AUTO_INCREMENT,
    FullName VARCHAR(50) NOT NULL,
    Email VARCHAR(50) NOT NULL UNIQUE,
    RegistrationDate DATE NOT NULL
);

INSERT INTO Students (FullName, Email, RegistrationDate)
VALUES
('John Doe', 'johndoe@example.com', '2024-01-15'),
-- Additional sample data...
('Sophia Walker', 'sophiawalker@example.com', '2024-10-15');
```

#### Courses Table

```sql
CREATE TABLE Courses (
    CourseID INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    InstructorID INT,
    Duration INT,
    Category VARCHAR(255),
    FOREIGN KEY (InstructorID) REFERENCES Instructors(InstructorID)
);

INSERT INTO Courses (CourseID, Name, InstructorID, Duration, Category)
VALUES
(1, 'Introduction to Programming', 1, 30, 'Programming'),
-- Additional sample data...
(15, 'Advanced Web Development', 15, 45, 'Programming');
```

#### Enrollments Table

```sql
CREATE TABLE Enrollments (
    EnrollmentID INT PRIMARY KEY,
    CourseID INT,
    StudentID INT,
    EnrollmentDate DATE,
    Progress DECIMAL(10, 3),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID),
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID)
);

INSERT INTO Enrollments (EnrollmentID, CourseID, StudentID, EnrollmentDate, Progress)
VALUES
(1, 1, 1, '2024-08-01', 75.12),
-- Additional sample data...
(15, 15, 15, '2024-07-30', 70.00);
```

### Operations

#### Add Category Column

```sql
ALTER TABLE Courses
ADD COLUMN Category VARCHAR(255);

UPDATE Courses
SET Category = CASE
    WHEN Name LIKE '%Programming%' THEN 'Programming'
    WHEN Name LIKE '%Data%' THEN 'Data Science'
    WHEN Name LIKE '%Cybersecurity%' THEN 'IT Security'
    ELSE 'Other'
END;
```

#### Remove Description Column

```sql
ALTER TABLE Courses
DROP COLUMN Description;
```

#### Update Progress

```sql
UPDATE Enrollments
SET Progress = 100
WHERE StudentID = (SELECT StudentID FROM Students WHERE FullName = 'Benjamin Harris');
```

#### Retrieve Enrollments for a Student

```sql
SELECT Courses.CourseID, Courses.Name, Courses.Duration
FROM Courses
INNER JOIN Enrollments ON Courses.CourseID = Enrollments.CourseID
INNER JOIN Students ON Enrollments.StudentID = Students.StudentID
WHERE Students.FullName = 'David Clark';
```

#### Delete a Student

```sql
DELETE FROM Enrollments
WHERE StudentID = 3;

DELETE FROM Students
WHERE StudentID = 3;
```

---

## Test Results

All queries were tested, and the database performed as expected, fulfilling the outlined requirements.

---

## How to Use

1. Clone this repository.
2. Run the scripts in a MySQL-compatible environment.
3. Adjust sample data or queries for your testing needs.

---
### Limitations
- Create a separate table for the instructors so the information can be easily managed
- Add more data points to make the database more robust in terms of analysis capacity
- For Students that are less than 18, let's have a table called parent and attach parent_id to such student details.

### Contributions and Feedback

Feel free to open issues or submit pull requests to improve this project.
