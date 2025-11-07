# CREATE TABLE

CREATE TABLE Enrollments (
StudentID INT NOT NULL,
CourseID INT NOT NULL,
PRIMARY KEY (StudentID, CourseID),
FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);

# SELECT

# WHERE 1

SELECT FirstName, LastName FROM Students
WHERE LastName LIKE "L%"
ORDER BY FirstName ASC LIMIT 10;

# WHERE 2

SELECT COUNT(\*) FROM Students
WHERE Email LIKE "%example.com"

# WHERE 3

SELECT \* FROM classrooms
WHERE Capacity >= 30;

## -----------------------

# JOINS 1

SELECT
t.TeacherID,
t.FirstName,
t.LastName,
COUNT(c.CourseID) AS CourseCount
FROM teachers AS t
LEFT JOIN Courses AS c ON c.TeacherID = t.TeacherID
GROUP BY t.TeacherID, t.FirstName, t.LastName
ORDER BY CourseCount DESC;

# JOINS 2

SELECT
t.FirstName,
t.LastName,
c.CourseName,
c.TeacherID
FROM teachers AS t
LEFT JOIN Courses AS c ON c.TeacherID = t.TeacherID

# JOINS 3

SELECT
s.FirstName,
c.CourseName
FROM Enrollments e
JOIN Students s ON e.StudentID = s.StudentID
JOIN Courses c ON e.CourseID = c.CourseID;
