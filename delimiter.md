### **What is `DELIMITER $$` in SQL?**
In **MySQL**, the `DELIMITER` command is used to change the default statement delimiter (`;`) to another string (e.g., `$$` or `//`). This is useful when defining **stored procedures, functions, or triggers**, which contain multiple SQL statements.

---

### **Why Do We Need `DELIMITER`?**
By default, MySQL uses `;` to terminate statements. However, when creating procedures or functions, we need to include multiple SQL statements inside a `BEGIN ... END` block. If we use `;`, MySQL might interpret it as the **end of the procedure** instead of just an internal statement.

To avoid confusion, we **temporarily change the delimiter** to something like `$$` or `//`, allowing MySQL to correctly parse the procedure.

---

### **Example Without `DELIMITER` (Error)**
```sql
CREATE PROCEDURE example_proc()
BEGIN
    SELECT 'Hello';  -- MySQL thinks this is the end of the procedure
    SELECT 'World';
END;
```
🔴 **Error:** MySQL will think the procedure ends at `SELECT 'Hello';`, causing a syntax error.

---

### **Example With `DELIMITER $$` (Correct)**
```sql
DELIMITER $$

CREATE PROCEDURE example_proc()
BEGIN
    SELECT 'Hello';
    SELECT 'World';
END $$

DELIMITER ;
```
✅ **Explanation:**
1. `DELIMITER $$` → Changes the delimiter from `;` to `$$`.
2. The `CREATE PROCEDURE` statement **uses `$$`** to indicate the end.
3. `DELIMITER ;` → Resets the delimiter back to `;` for normal SQL statements.

---

### **Executing the Procedure**
After creating the procedure, **call it using the normal `;` delimiter**:
```sql
CALL example_proc();
```

---

### **Alternative Delimiters**
Instead of `$$`, we can use **`//` or any other symbol**:
```sql
DELIMITER //
CREATE PROCEDURE test_proc()
BEGIN
    SELECT 'Test';
END //
DELIMITER ;
```

---

### **When to Use `DELIMITER`**
- When defining **stored procedures**.
- When creating **functions**.
- When writing **triggers**.
- When working with **BEGIN ... END** blocks that contain multiple SQL statements.

---

### **Key Takeaways**
✔ `DELIMITER` **is not an SQL command**, but a MySQL-specific instruction for handling multi-statement blocks.  
✔ It **avoids conflicts** with the default `;` separator.  
✔ Always **reset the delimiter** back to `;` after defining the procedure.  

