# Basic  rules of SQL commands
## 1. SQL Keywords and Case Sensitivity
## 2. SQL Statements Must End with a Semicolon (;)
## 3. String and Numeric Values
✔ Strings should be enclosed in single quotes (') ,Numbers do not need quotes
## 4. Table and Column Naming Rules
✔ Identifiers (table names, column names) should be meaningful

Bad: t1, c1
Good: employees, salary, join_date
✔ Use backticks ( ) for special characters or reserved keywords

sql
Copy
Edit
SELECT `first-name` FROM `user-data`;
✔ Avoid using SQL Reserved Keywords as table or column names

Reserved words like SELECT, ORDER, DATE should not be used as column names.
