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

# 2.what is the difference between char and varchar?
The difference between `CHAR` and `VARCHAR` in database management systems lies in how they store and manage data:

- **`CHAR` (Fixed Length):**
  - Always stores data in a fixed length.
  - If the data is shorter than the defined length, the remaining space is padded with spaces.
  - Useful when the data entries are of a consistent size, for example, a country code or a state abbreviation.
  - Slightly faster in terms of performance because of its fixed length.

- **`VARCHAR` (Variable Length):**
  - Stores data in a variable length.
  - Does not pad with extra spaces, saving storage space.
  - Useful when the size of the data entries can vary significantly, for example, names or addresses.
  - Can potentially lead to slightly slower performance compared to `CHAR` because the length is not fixed.

**Example:**
```sql
CREATE TABLE ExampleTable (
  fixedColumn CHAR(10),
  variableColumn VARCHAR(10)
);
```

In this example, `fixedColumn` is a `CHAR` field that will always take up 10 characters, even if the data is shorter, while `variableColumn` is a `VARCHAR` field that can store up to 10 characters but only uses as much space as needed for the data.
# 3.How do you copy the schema of a table?
To copy the schema of a table without copying the data, you can use the `CREATE TABLE` statement with the `LIKE` clause in SQL. This creates a new table with the same structure (columns, data types, constraints) as the existing table.

Here's how you can do it:

```sql
CREATE TABLE new_table LIKE existing_table;
```

In this example:
- `new_table` is the name of the new table you want to create.
- `existing_table` is the name of the existing table whose schema you want to copy.

This command will create `new_table` with the same column definitions and indexes as `existing_table`, but without any data.

# 4.  Why this query will not work?
select name from Customer where referee_id !=2 or referee_id = null;  
Your SQL query has a **logical mistake** in the condition `referee_id = null`.  

### **Issues in Your Query:**
1. **Incorrect NULL check**:  
   - In SQL, `= NULL` does **not** work because `NULL` is **not a value**, but rather a placeholder for missing data.  
   - Instead, use `IS NULL` to check for `NULL` values.

2. **Logical issue with `!= 2 OR referee_id = NULL`**:  
   - The condition `referee_id != 2` will **not** include `NULL` values, because any comparison with `NULL` results in `UNKNOWN`.
   - The correct way to include `NULL` values is by using `IS NULL`.

---

### **Corrected Query**
```sql
SELECT name FROM Customer WHERE referee_id != 2 OR referee_id IS NULL;
```

### **Fixes:**
✅ **Replaced** `referee_id = NULL` with `referee_id IS NULL`.  
✅ **Keeps records** where `referee_id` is either **not 2** or **NULL**.  



