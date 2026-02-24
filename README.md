# employees-attendance-dashboard
Employee attendance data analysis using SQL to identify absentee patterns and workforce trends.
# 📊 Employee Attendance Dashboard (SQL Project)

## 📌 Project Overview
This project analyzes employee attendance and leave data using SQL to identify absenteeism trends, department performance, and workforce patterns.

---

## 🖼 ER Diagram
Database Schema[<img width="948" height="694" alt="Screenshot 2026-02-24 at 11 24 49 AM" src="https://github.com/user-attachments/assets/773b18d0-ebd3-4e92-898a-eb97e83ec0db" />](https://private-user-images.githubusercontent.com/263546743/553923459-773b18d0-ebd3-4e92-898a-eb97e83ec0db.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NzE5MTMxNTMsIm5iZiI6MTc3MTkxMjg1MywicGF0aCI6Ii8yNjM1NDY3NDMvNTUzOTIzNDU5LTc3M2IxOGQwLWViZDMtNGU5Mi04OThhLWViOTdlODNlYzBkYi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYwMjI0JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MDIyNFQwNjAwNTNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05YTA4OTgwMTY5MTg2NDk0NzUwOGRiOGI2ZjIzZjkyMmYxMjA4NzhlMzBmZGM2NTYyMzEzYWYxMWU3MzQxOWFmJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.BJxLp6i-irY5KRxGaD16YoASmYDzyA7uZYSzllZIzYI))

---

## 🗄 Database Schema

### EMPLOYEES
- EMP_ID (Primary Key)
- EMP_NAME
- DEPARTMENT
- JOIN_DATE

### ATTENDANCE
- ATTENDANCE_ID (Primary Key)
- EMP_ID (Foreign Key)
- ATTENDANCE_DATE
- STATUS (Present / Absent / Late)

### LEAVE_REQUEST
- LEAVE_ID (Primary Key)
- EMP_ID (Foreign Key)
- LEAVE_DATE
- REASON
- STATUS (Pending / Approved / Rejected)

Relationship:
- One employee → Many attendance records
- One employee → Many leave requests

---

## 🛠 Tools Used
- MySQL
- SQL (JOIN, GROUP BY, Aggregations)

---

## 📈 Key Analysis Performed

- Monthly attendance percentage
- Employees with highest absenteeism
- Department-wise attendance summary
- Leave approval trend analysis

---

## 💡 Business Insights

- Identified departments with lower attendance rates
- Detected employees with frequent absences
- Recommended monitoring strategy for HR
---

## 📂 Project Files Explanation

### 1️⃣ schema.sql
Contains all table creation queries including:
- EMPLOYEES table
- ATTENDANCE table
- LEAVE_REQUEST table
Defines primary keys and foreign key relationships.

### 2️⃣ sample_data.sql
Includes INSERT statements to populate the tables with sample employee, attendance, and leave data for testing and analysis.

### 3️⃣ analysis_queries.sql
Contains SQL queries used for data analysis such as:
- Monthly attendance percentage
- Department-wise attendance summary
- Employees with highest absenteeism
- Leave status trend analysis

### 4️⃣ er-diagram.png
Visual representation of the database structure showing table relationships and one-to-many connections.


## Employees Table 
<img width="703" height="473" alt="Screenshot 2026-02-24 at 11 26 49 AM" src="https://github.com/user-attachments/assets/8c454aff-67c5-412d-be3c-8a4462b25e37" />

## Attendance Table
<img width="637" height="508" alt="Screenshot 2026-02-24 at 11 29 24 AM" src="https://github.com/user-attachments/assets/a4f14055-c7ee-43f8-a14d-eb41b22952ec" />

## Leave Request Table
<img width="649" height="448" alt="Screenshot 2026-02-24 at 11 30 39 AM" src="https://github.com/user-attachments/assets/7f1cd7e5-fc0c-4a44-87c0-5e085902eedf" />

## Employee Attendance Analysis Queries

## 1️⃣ Monthly Attendance Percentage
SELECT 
    DATE_FORMAT(attendance_date, '%Y-%m') AS month,
    ROUND(
        (SUM(CASE WHEN status = 'Present' THEN 1 ELSE 0 END) 
        / COUNT(*)) * 100, 2
    ) AS attendance_percentage
FROM attendance
GROUP BY month
ORDER BY month;

## 2️⃣ Employees with Highest Absenteeism
SELECT 
    e.emp_name,
    COUNT(a.attendance_id) AS total_absent_days
FROM employees e
JOIN attendance a ON e.emp_id = a.emp_id
WHERE a.status = 'Absent'
GROUP BY e.emp_name
ORDER BY total_absent_days DESC
LIMIT 5;

## 3️⃣ Department-wise Attendance Summary
SELECT 
    e.department,
    COUNT(a.attendance_id) AS total_records,
    SUM(CASE WHEN a.status = 'Present' THEN 1 ELSE 0 END) AS total_present
FROM employees e
JOIN attendance a ON e.emp_id = a.emp_id
GROUP BY e.department;
## 4️⃣ Leave Approval Trend
SELECT 
    status,
    COUNT(*) AS total_requests
FROM leave_request
GROUP BY status;

## 5️⃣ Employee Attendance Rate
SELECT 
    e.emp_name,
    ROUND(
        (SUM(CASE WHEN a.status = 'Present' THEN 1 ELSE 0 END)
        / COUNT(*)) * 100, 2
    ) AS attendance_rate
FROM employees e
JOIN attendance a ON e.emp_id = a.emp_id
GROUP BY e.emp_name
ORDER BY attendance_rate DESC;

## Employees with Attendance Rate Below 80%

SELECT 
    e.emp_name,
    ROUND(
        (SUM(CASE WHEN a.status = 'Present' THEN 1 ELSE 0 END)
        / COUNT(*)) * 100, 2
    ) AS attendance_rate
FROM employees e
JOIN attendance a ON e.emp_id = a.emp_id
GROUP BY e.emp_name
HAVING attendance_rate < 80;


## 📌 Conclusion

This project demonstrates SQL-based data analysis including joins, aggregations, trend analysis, and business insight generation for workforce management.

The structured database design and analytical queries help HR teams monitor attendance performance and identify absenteeism patterns.
