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
