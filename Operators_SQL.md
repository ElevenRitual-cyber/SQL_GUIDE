
- **Relational Operators** (such as =, <, >, <=, >=, <>) and **Logical Operators** (AND, OR, NOT) are predominantly used in the **WHERE clause** of SQL queries to filter records based on specific conditions.
- These operators enable comparisons and combine conditions to determine which rows should be returned by a query.
- Although the WHERE clause is the most common context for these operators, they are also used in other SQL clauses like **HAVING** (for filtering groups) and in **CASE expressions** for conditional logic.

### **Operators in SQL**  
SQL provides various operators to perform operations on data. These operators are classified into different categories.

---

## **1. Arithmetic Operators**  
Used for mathematical calculations.

| Operator | Description       | Example (`a = 10`, `b = 5`) |
|----------|------------------|----------------------------|
| `+`      | Addition         | `a + b = 15`              |
| `-`      | Subtraction      | `a - b = 5`               |
| `*`      | Multiplication   | `a * b = 50`              |
| `/`      | Division         | `a / b = 2`               |
| `%`      | Modulus (Remainder) | `a % b = 0`          |

**Example Query:**
```sql
SELECT 10 + 5 AS Sum, 10 - 5 AS Difference;
```

---

## **2. Comparison (Relational) Operators**  
Used to compare values in SQL queries.

| Operator | Description                  | Example (`a = 10`, `b = 5`) |
|----------|------------------------------|-----------------------------|
| `=`      | Equal to                     | `a = b` → `false`           |
| `!=` or `<>` | Not equal to          | `a != b` → `true`          |
| `>`      | Greater than                 | `a > b` → `true`            |
| `<`      | Less than                    | `a < b` → `false`           |
| `>=`     | Greater than or equal to     | `a >= b` → `true`           |
| `<=`     | Less than or equal to        | `a <= b` → `false`          |

**Example Query:**
```sql
SELECT * FROM Employees WHERE Salary > 50000;
```

---

## **3. Logical Operators**  
Used to combine multiple conditions.

| Operator | Description                | Example (`A = true`, `B = false`) |
|----------|----------------------------|----------------------------------|
| `AND`    | Returns true if both conditions are true | `A AND B` → `false` |
| `OR`     | Returns true if at least one condition is true | `A OR B` → `true` |
| `NOT`    | Reverses the condition     | `NOT A` → `false` |

**Example Query:**
```sql
SELECT * FROM Employees WHERE Salary > 50000 AND Department = 'IT';
```

---

## **4. Special Operators**  
Additional SQL operators used in queries.

| Operator | Description | Example |
|----------|------------|---------|
| `BETWEEN` | Checks if value is within a range | `Salary BETWEEN 30000 AND 50000` |
| `IN` | Checks if value matches a set of values | `Department IN ('HR', 'IT', 'Sales')` |
| `LIKE` | Searches for a pattern in a string | `Name LIKE 'A%'` (names starting with 'A') |
| `NOT IN` | Checks IF values does not matches a set of values | `Address NOT IN` |

| Operator  | Description                                  | Example Query |
|-----------|----------------------------------------------|--------------|
| `IS NULL`   | Checks if a column has NULL values         | `SELECT * FROM Employees WHERE Address IS NULL;` |
| `IS NOT NULL` | Checks if a column does **not** have NULL values | `SELECT * FROM Employees WHERE Address IS NOT NULL;` |

---

### **Example Usage**
#### 1️⃣ **Checking for NULL values**
```sql
SELECT * FROM Employees WHERE Address IS NULL;
```
✅ This query retrieves all employees who have a NULL address.

#### 2️⃣ **Checking for NOT NULL values**
```sql
SELECT * FROM Employees WHERE Address IS NOT NULL;
```
✅ This query retrieves all employees who have a valid (non-NULL) address.

---

### ❗ **Important Notes About NULL in SQL**
- **NULL is not equal to 0 or an empty string (`''`)**.
- `IS NULL` and `IS NOT NULL` must be used instead of `=` or `!=`.
- Using `NULL` in mathematical expressions results in `NULL` (`10 + NULL = NULL`).
- Aggregation functions (like `SUM()`, `AVG()`) ignore `NULL` values unless explicitly handled.



**Example Query:**
```sql
SELECT * FROM Employees WHERE Name LIKE 'J%';
```

---

## **5. Assignment Operator**  
Used to assign a value to a variable.

| Operator | Description | Example |
|----------|------------|---------|
| `=`      | Assigns a value | `SET @name = 'John';` |

**Example Query:**
```sql
SET @total_salary = (SELECT SUM(Salary) FROM Employees);
```

---

### **Conclusion**  
SQL operators help in performing calculations, comparisons, logical operations, and filtering data in queries efficiently.  

Let me know if you need further clarification! 🚀
