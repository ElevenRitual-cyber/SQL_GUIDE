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
