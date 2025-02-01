# 1.How would you copy data from existing table into another table?
# To create a new table and copy the existing data from another table, you can use the `CREATE TABLE ... AS SELECT` syntax. 

### **Syntax:**
```sql
CREATE TABLE new_table_name AS SELECT * FROM existing_table_name;
```

### **Explanation:**
- `new_table_name`: The name of the new table you want to create.
- `existing_table_name`: The name of the table from which data will be copied.
- The `SELECT *` will select all columns from the existing table and insert them into the newly created table.

### **Example:**
Suppose you have an existing table `employees` and you want to create a new table `employees_backup` that contains all the data from the `employees` table:

```sql
CREATE TABLE employees_backup AS SELECT * FROM employees;
```

### **Points to Note:**
- The new table (`employees_backup` in this case) will have the same data as the `employees` table, but the table structure (such as constraints, primary keys, etc.) is not copied.
- If you want to include only specific columns, you can modify the `SELECT` query:
  
  ```sql
  CREATE TABLE employees_backup AS SELECT emp_id, emp_name, salary FROM employees;
  ```

This will copy only the `emp_id`, `emp_name`, and `salary` columns into the new `employees_backup` table.
