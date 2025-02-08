### 1.**What is Pagination in SQL?** (LIMIT Clause)
Pagination is a technique used to divide a large dataset into smaller, manageable chunks (pages). It is commonly used in applications where users browse records, such as **product listings, search results, or user profiles**.
## Note : LIMIT clause is always used at the end of the query before the semincolon;
---

### **How Does Pagination Work in SQL?**
Pagination typically involves the `LIMIT` and `OFFSET` clauses:

- **`LIMIT N`** → Fetches at most `N` rows.
- **`OFFSET M`** → Skips the first `M` rows.

#### **Basic Syntax**
```sql
SELECT * FROM table_name
ORDER BY column_name
LIMIT page_size OFFSET offset_value;
```
- **`page_size`** → Number of rows per page.
- **`offset_value`** → The starting row to fetch.

---

### **Example: Employee Table**
```
+----+----------+--------+
| ID | Name     | Salary |
+----+----------+--------+
| 1  | Alice    | 50000  |
| 2  | Bob      | 55000  |
| 3  | Charlie  | 60000  |
| 4  | David    | 65000  |
| 5  | Eve      | 70000  |
| 6  | Frank    | 75000  |
| 7  | Grace    | 80000  |
+----+----------+--------+
```

#### **Page 1 (First 3 Records)**
```sql
SELECT * FROM Employees
ORDER BY ID
LIMIT 3 OFFSET 0;
```
**Output:**
```
+----+----------+--------+
| 1  | Alice    | 50000  |
| 2  | Bob      | 55000  |
| 3  | Charlie  | 60000  |
+----+----------+--------+
```

#### **Page 2 (Next 3 Records)**
```sql
SELECT * FROM Employees
ORDER BY ID
LIMIT 3 OFFSET 3;
```
**Output:**
```
+----+----------+--------+
| 4  | David    | 65000  |
| 5  | Eve      | 70000  |
| 6  | Frank    | 75000  |
+----+----------+--------+
```

---

### **Formula for Pagination**
To fetch **page `n`**:
```sql
OFFSET = (page_number - 1) * page_size
```
For example:
- **Page 1:** `LIMIT 3 OFFSET 0`
- **Page 2:** `LIMIT 3 OFFSET 3`
- **Page 3:** `LIMIT 3 OFFSET 6`

---

### **Alternative: Keyset Pagination (Better Performance)**
Instead of using `OFFSET`, **Keyset Pagination** is faster:
```sql
SELECT * FROM Employees
WHERE ID > last_seen_id
ORDER BY ID
LIMIT 3;
```
- This avoids skipping rows, making it **more efficient**.

---

### **Performance Considerations**
1. **`OFFSET` can be slow on large datasets**  
   - Instead, use **Keyset Pagination** (`WHERE id > last_seen_id`).
2. **Indexes improve performance**  
   - Ensure an index exists on `ORDER BY` columns.

### 2.**What is the `ORDER BY` Clause in SQL?**  (ORDER BY Clause)
The `ORDER BY` clause is used to **sort the result set** of a query in **ascending (ASC)** or **descending (DESC)** order based on one or more columns.

---

### **Syntax**
```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column_name [ASC|DESC];
```
- **`ASC` (default)** → Sorts results in **ascending order** (lowest to highest).
- **`DESC`** → Sorts results in **descending order** (highest to lowest).

---

### **Example: Sorting by One Column**
Consider the `Employees` table:
```
+----+----------+--------+
| ID | Name     | Salary |
+----+----------+--------+
| 1  | Alice    | 50000  |
| 2  | Bob      | 55000  |
| 3  | Charlie  | 60000  |
| 4  | David    | 65000  |
| 5  | Eve      | 70000  |
+----+----------+--------+
```

#### **Sort by Salary (Ascending)**
```sql
SELECT * FROM Employees ORDER BY Salary;
```
**Output:**
```
+----+----------+--------+
| 1  | Alice    | 50000  |
| 2  | Bob      | 55000  |
| 3  | Charlie  | 60000  |
| 4  | David    | 65000  |
| 5  | Eve      | 70000  |
+----+----------+--------+
```

#### **Sort by Salary (Descending)**
```sql
SELECT * FROM Employees ORDER BY Salary DESC;
```
**Output:**
```
+----+----------+--------+
| 5  | Eve      | 70000  |
| 4  | David    | 65000  |
| 3  | Charlie  | 60000  |
| 2  | Bob      | 55000  |
| 1  | Alice    | 50000  |
+----+----------+--------+
```

---

### **Sorting by Multiple Columns**
If multiple employees have the same salary, you can **break ties** by adding another column.

#### **Sort by Salary (Descending) & Name (Ascending)**
```sql
SELECT * FROM Employees ORDER BY Salary DESC, Name ASC;
```
- First, sorts by **Salary** in descending order.
- If salaries are the same, sorts by **Name** in ascending order.

---

### **Using `ORDER BY` with `LIMIT` (Pagination)**
You can combine `ORDER BY` with `LIMIT` to **fetch top results**.

#### **Get Top 3 Highest Paid Employees**
```sql
SELECT * FROM Employees ORDER BY Salary DESC LIMIT 3;
```
**Output:**
```
+----+----------+--------+
| 5  | Eve      | 70000  |
| 4  | David    | 65000  |
| 3  | Charlie  | 60000  |
+----+----------+--------+
```

---

### **Key Takeaways**
✔ `ORDER BY` sorts query results based on one or more columns.  
✔ Defaults to **ascending order** (`ASC`).  
✔ Use `DESC` for **descending order**.  
✔ Supports **multiple columns** for advanced sorting.  
✔ Works well with `LIMIT` for **pagination**.  
### **What is `DISTINCT` in SQL?**  
The `DISTINCT` keyword is used in SQL to **remove duplicate values** from a result set and return only **unique** values.

---

### 3.**Where is `DISTINCT` Used?**  
`DISTINCT` is commonly used in:
1. **Fetching unique values from a column**
2. **Avoiding duplicate records**
3. **Getting unique combinations of multiple columns**

---

### **Syntax**
```sql
SELECT DISTINCT column_name FROM table_name;
```
or  
```sql
SELECT DISTINCT column1, column2 FROM table_name;
```

---

### **Example 1: Fetching Unique Values**
#### **Table: Employees**
```
+----+----------+------------+
| ID | Name     | Department |
+----+----------+------------+
| 1  | Alice    | HR         |
| 2  | Bob      | IT         |
| 3  | Charlie  | HR         |
| 4  | David    | IT         |
| 5  | Eve      | HR         |
+----+----------+------------+
```
#### **Get Unique Departments**
```sql
SELECT DISTINCT Department FROM Employees;
```
**Output:**
```
+------------+
| Department |
+------------+
| HR         |
| IT         |
+------------+
```
🔹 Here, duplicate `HR` and `IT` values are removed.

---

### **Example 2: Fetching Unique Combinations**
If you apply `DISTINCT` on **multiple columns**, it returns **unique row combinations**.

```sql
SELECT DISTINCT Name, Department FROM Employees;
```
**Output:**
```
+----------+------------+
| Name     | Department |
+----------+------------+
| Alice    | HR         |
| Bob      | IT         |
| Charlie  | HR         |
| David    | IT         |
| Eve      | HR         |
+----------+------------+
```
Since all **name-department** pairs are unique, it returns all rows.

---

### **Example 3: Using `DISTINCT` with `COUNT`**
If you want to count unique values:
```sql
SELECT COUNT(DISTINCT Department) FROM Employees;
```
**Output:**
```
+------------------------+
| COUNT(DISTINCT Department) |
+------------------------+
| 2                      |
+------------------------+
```

---

### **Key Takeaways**
✔ `DISTINCT` removes **duplicates** from a result set.  
✔ Works with **single or multiple columns**.  
✔ Can be used with **aggregate functions** like `COUNT()`.  
✔ **Does not modify the actual table**, just affects query results.  
# 4. ### **GROUP BY and HAVING Clauses in SQL**

#### **1. GROUP BY Clause**
The `GROUP BY` clause in SQL is used to group rows that have the same values in specified columns into summary rows. It is often used with aggregate functions (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`, etc.) to perform calculations on each group.

**Syntax:**
```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name;
```

**Example:**
Suppose we have a `Sales` table:

| OrderID | Product | Category   | Amount |
|---------|---------|-----------|--------|
| 1       | Laptop  | Electronics | 1000   |
| 2       | Phone   | Electronics | 500    |
| 3       | Shirt   | Clothing   | 200    |
| 4       | Laptop  | Electronics | 1200   |
| 5       | Shirt   | Clothing   | 300    |

To find the total sales amount for each category:
```sql
SELECT Category, SUM(Amount) AS TotalSales
FROM Sales
GROUP BY Category;
```

**Output:**
| Category   | TotalSales |
|-----------|------------|
| Electronics | 2700       |
| Clothing    | 500        |

---

#### **2. HAVING Clause**
The `HAVING` clause is used to filter groups after they have been created by `GROUP BY`. Unlike `WHERE`, which filters individual rows before aggregation, `HAVING` filters groups after aggregation.

**Syntax:**
```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

**Example:**
To find only those categories where total sales exceed 1000:
```sql
SELECT Category, SUM(Amount) AS TotalSales
FROM Sales
GROUP BY Category
HAVING SUM(Amount) > 1000;
```

**Output:**
| Category   | TotalSales |
|-----------|------------|
| Electronics | 2700       |

---

### **Key Differences: GROUP BY vs HAVING**
| Feature     | GROUP BY | HAVING |
|------------|---------|--------|
| Purpose    | Groups rows based on column values | Filters groups after aggregation |
| Used With  | Aggregate functions & non-aggregated columns | Aggregate functions |
| Execution  | Happens before HAVING | Applied after GROUP BY |
| Can Use WHERE? | Yes, before GROUP BY | No, only after GROUP BY |

---

### **Example Combining WHERE, GROUP BY, and HAVING**
```sql
SELECT Category, COUNT(*) AS ProductCount, SUM(Amount) AS TotalSales
FROM Sales
WHERE Amount > 200  -- Filters before grouping
GROUP BY Category
HAVING SUM(Amount) > 1000; -- Filters after grouping
```
This query:
1. Filters out rows where `Amount ≤ 200` (using `WHERE`).
2. Groups remaining rows by `Category`.
3. Filters groups where total sales exceed `1000` (using `HAVING`).

   
# **4.GROUP BY and HAVING Clauses in SQL**

#### **1. GROUP BY Clause**
The `GROUP BY` clause in SQL is used to group rows that have the same values in specified columns into summary rows. It is often used with aggregate functions (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`, etc.) to perform calculations on each group.

**Syntax:**
```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name;
```

**Example:**
Suppose we have a `Sales` table:

| OrderID | Product | Category   | Amount |
|---------|---------|-----------|--------|
| 1       | Laptop  | Electronics | 1000   |
| 2       | Phone   | Electronics | 500    |
| 3       | Shirt   | Clothing   | 200    |
| 4       | Laptop  | Electronics | 1200   |
| 5       | Shirt   | Clothing   | 300    |

To find the total sales amount for each category:
```sql
SELECT Category, SUM(Amount) AS TotalSales
FROM Sales
GROUP BY Category;
```

**Output:**
| Category   | TotalSales |
|-----------|------------|
| Electronics | 2700       |
| Clothing    | 500        |

---

#### **2. HAVING Clause**
The `HAVING` clause is used to filter groups after they have been created by `GROUP BY`. Unlike `WHERE`, which filters individual rows before aggregation, `HAVING` filters groups after aggregation.

**Syntax:**
```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

**Example:**
To find only those categories where total sales exceed 1000:
```sql
SELECT Category, SUM(Amount) AS TotalSales
FROM Sales
GROUP BY Category
HAVING SUM(Amount) > 1000;
```

**Output:**
| Category   | TotalSales |
|-----------|------------|
| Electronics | 2700       |

---

### **Key Differences: GROUP BY vs HAVING**
| Feature     | GROUP BY | HAVING |
|------------|---------|--------|
| Purpose    | Groups rows based on column values | Filters groups after aggregation |
| Used With  | Aggregate functions & non-aggregated columns | Aggregate functions |
| Execution  | Happens before HAVING | Applied after GROUP BY |
| Can Use WHERE? | Yes, before GROUP BY | No, only after GROUP BY |

---

### **Example Combining WHERE, GROUP BY, and HAVING**
```sql
SELECT Category, COUNT(*) AS ProductCount, SUM(Amount) AS TotalSales
FROM Sales
WHERE Amount > 200  -- Filters before grouping
GROUP BY Category
HAVING SUM(Amount) > 1000; -- Filters after grouping
```
This query:
1. Filters out rows where `Amount ≤ 200` (using `WHERE`).
2. Groups remaining rows by `Category`.
3. Filters groups where total sales exceed `1000` (using `HAVING`).

## 5. **CASE WHEN THEN in SQL**
The `CASE` statement in SQL is used to implement conditional logic within queries. It works like an `IF-ELSE` statement in programming, allowing you to return different values based on specified conditions.

---

### **Syntax**
```sql
SELECT 
    column_name,
    CASE 
        WHEN condition1 THEN result1
        WHEN condition2 THEN result2
        ELSE default_result
    END AS alias_name
FROM table_name;
```
- `WHEN condition THEN result`: If the condition is met, the corresponding result is returned.
- `ELSE default_result`: If no conditions are met, the default value is returned.
- `END AS alias_name`: Assigns a name to the computed column.

---

### **Example 1: Categorizing Data**
#### **Scenario:** You have an `Employees` table.

| EmployeeID | Name  | Salary |
|------------|------|--------|
| 1          | John  | 5000   |
| 2          | Alice | 8000   |
| 3          | Bob   | 12000  |
| 4          | Eve   | 3000   |

#### **Query: Classify employees based on salary**
```sql
SELECT 
    Name, 
    Salary,
    CASE 
        WHEN Salary < 5000 THEN 'Low'
        WHEN Salary BETWEEN 5000 AND 10000 THEN 'Medium'
        ELSE 'High'
    END AS SalaryCategory
FROM Employees;
```

#### **Output:**
| Name  | Salary | SalaryCategory |
|------|--------|---------------|
| John  | 5000   | Medium        |
| Alice | 8000   | Medium        |
| Bob   | 12000  | High          |
| Eve   | 3000   | Low           |

---

### **Example 2: Using CASE in ORDER BY**
If you want to sort employees by salary category, you can use `CASE` inside `ORDER BY`:

```sql
SELECT Name, Salary
FROM Employees
ORDER BY 
    CASE 
        WHEN Salary < 5000 THEN 1
        WHEN Salary BETWEEN 5000 AND 10000 THEN 2
        ELSE 3
    END;
```
This will order the results as:
1. Low salary first (`<5000`).
2. Medium salary (`5000-10000`).
3. High salary (`>10000`).

---

### **Example 3: Using CASE in Aggregation**
You can use `CASE` inside aggregate functions:

```sql
SELECT 
    COUNT(CASE WHEN Salary < 5000 THEN 1 END) AS LowIncomeEmployees,
    COUNT(CASE WHEN Salary BETWEEN 5000 AND 10000 THEN 1 END) AS MediumIncomeEmployees,
    COUNT(CASE WHEN Salary > 10000 THEN 1 END) AS HighIncomeEmployees
FROM Employees;
```

#### **Output:**
| LowIncomeEmployees | MediumIncomeEmployees | HighIncomeEmployees |
|--------------------|----------------------|---------------------|
| 1                 | 2                     | 1                   |

---

### **Key Takeaways**
✅ `CASE` is useful for conditional logic in SQL.  
✅ It can be used in `SELECT`, `WHERE`, `ORDER BY`, and even aggregate functions.  
✅ Always use `END` to close the `CASE` statement.  
✅ The `ELSE` part is optional but recommended to avoid `NULL` values.

# 6. **COALESCE in SQL**
The `COALESCE` function returns the **first non-NULL** value from a list of expressions. It is commonly used to handle `NULL` values in queries.

---

### **Syntax**
```sql
COALESCE(value1, value2, ..., valueN)
```
- Evaluates expressions **from left to right**.
- Returns the **first non-NULL** value it encounters.
- If all values are `NULL`, it returns `NULL`.

---

### **Can COALESCE Take Multiple Values?**
Yes! `COALESCE` can take multiple values. It evaluates them **in order** and returns the first non-NULL value.

---

### **Example 1: Handling NULL Values in a Contact List**
#### **Scenario:** You have a `Customers` table with `PhoneNumber`, `Email`, and `Address`. Some values may be `NULL`.

| CustomerID | Name  | PhoneNumber | Email         | Address      |
|------------|------|------------|--------------|-------------|
| 1          | John  | NULL       | john@mail.com | NULL        |
| 2          | Alice | 9876543210 | NULL         | NULL        |
| 3          | Bob   | NULL       | NULL         | No Address  |
| 4          | Eve   | NULL       | NULL         | NULL        |

#### **Query: Return the first available contact method**
```sql
SELECT 
    Name,
    COALESCE(PhoneNumber, Email, Address, 'No Contact Available') AS ContactInfo
FROM Customers;
```

#### **Evaluation Order for Each Row:**
- **John**: `PhoneNumber` is `NULL`, so it checks `Email` → **returns "john@mail.com"**.
- **Alice**: `PhoneNumber` is not `NULL` → **returns "9876543210"**.
- **Bob**: `PhoneNumber` and `Email` are `NULL`, but `Address` is not → **returns "No Address"**.
- **Eve**: All fields are `NULL`, so it returns the default → **"No Contact Available"**.

#### **Output:**
| Name  | ContactInfo       |
|------|------------------|
| John  | john@mail.com   |
| Alice | 9876543210      |
| Bob   | No Address      |
| Eve   | No Contact Available |

---

### **Example 2: Handling Discounts in a Products Table**
| ProductID | Name  | SpecialDiscount | RegularDiscount | DefaultDiscount |
|-----------|------|----------------|----------------|----------------|
| 1         | Laptop  | NULL          | 10             | 5              |
| 2         | Phone   | 15            | NULL           | 5              |
| 3         | Tablet  | NULL          | NULL           | 5              |
| 4         | Headset | NULL          | NULL           | NULL           |

#### **Query: Get the First Available Discount**
```sql
SELECT 
    Name,
    COALESCE(SpecialDiscount, RegularDiscount, DefaultDiscount, 0) AS FinalDiscount
FROM Products;
```

#### **Evaluation Order for Each Row:**
1. **Laptop**: `SpecialDiscount` is `NULL`, so it checks `RegularDiscount` → **returns 10**.
2. **Phone**: `SpecialDiscount` is 15 (not `NULL`) → **returns 15**.
3. **Tablet**: Both `SpecialDiscount` and `RegularDiscount` are `NULL`, so it checks `DefaultDiscount` → **returns 5**.
4. **Headset**: All values are `NULL`, so it returns the default **0**.

#### **Output:**
| Name    | FinalDiscount |
|---------|--------------|
| Laptop  | 10           |
| Phone   | 15           |
| Tablet  | 5            |
| Headset | 0            |

---

### **Key Takeaways**
✅ **COALESCE takes multiple values** and returns the **first non-NULL** value.  
✅ Evaluates **left to right** and stops once it finds a non-NULL value.  
✅ If **all values are NULL**, it returns `NULL` (or a default value if provided).  
✅ Works in `SELECT`, `WHERE`, `ORDER BY`, and other SQL clauses.


## 7. Over and Partition Clauses:
---

## OVER Clause

- **Definition:**  
  The `OVER` clause is used with window functions to define the set of rows (the "window") that the function should operate on. Unlike aggregate functions combined with `GROUP BY`, window functions with `OVER` retain the individual rows of the result set while performing calculations across a set of rows.

- **Example Without PARTITION BY:**  
  Imagine you want to calculate a running total of sales over time.

  ```sql
  SELECT 
      sale_date,
      sale_amount,
      SUM(sale_amount) OVER (ORDER BY sale_date) AS running_total
  FROM sales;
  ```
  **Explanation:**  
  - `SUM(sale_amount)` is the window function.
  - `OVER (ORDER BY sale_date)` tells SQL to calculate the cumulative sum ordered by `sale_date`.  
  - The result includes every row from the `sales` table, with an extra column showing the running total.

---

## PARTITION BY Clause

- **Definition:**  
  The `PARTITION BY` clause is an option within the `OVER` clause that divides the result set into partitions (or groups) based on one or more columns. The window function is then applied independently to each partition.

- **Example With PARTITION BY:**  
  Suppose you want to calculate the average salary for employees within each department without collapsing the rows into a single summary row per department.

  ```sql
  SELECT 
      department,
      employee_name,
      salary,
      AVG(salary) OVER (PARTITION BY department) AS avg_salary_by_dept
  FROM employees;
  ```
  **Explanation:**  
  - `AVG(salary)` is the window function.
  - `OVER (PARTITION BY department)` tells SQL to calculate the average salary separately for each `department`.
  - Every row still appears in the final result, but each row now has an additional column showing the average salary for its department.

---

## Combined Example: PARTITION BY with ORDER BY

You can also combine `PARTITION BY` and `ORDER BY` within the `OVER` clause. For instance, if you want to rank employees within each department based on their salary:

```sql
SELECT 
    department,
    employee_name,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS salary_rank
FROM employees;
```

**Explanation:**  
- `RANK()` is the window function that assigns a rank to each row.
- `PARTITION BY department` divides the dataset into groups by department.
- `ORDER BY salary DESC` orders employees within each department by their salary in descending order.
- Each employee gets a rank relative to others in the same department.

---

## Summary

- **OVER:** Specifies the window (set of rows) over which a window function operates.
- **PARTITION BY:** An option inside the `OVER` clause that divides the result set into partitions, so that the window function is applied to each partition independently.
- **ORDER BY within OVER:** Further defines the order of rows within each partition, which is useful for functions that depend on order (like ranking or cumulative sums).

These tools allow you to perform complex calculations over subsets of your data while still retaining row-level details in your result set.
