## Views in SQL

Views are a special version of tables in SQL. They provide a **virtual table** environment for various complex operations. You can select data from multiple tables, or you can select specific data based on certain criteria in views.

### Characteristics of Views:
- **No Physical Storage:** A view does not hold the actual data; it holds only the definition of the view in the data dictionary.
- **Stored Query:** The view is a query stored in the data dictionary, on which the user can query just like they do on tables.
- **No Physical Memory Usage:** It does not use physical memory, only the query is stored in the data dictionary.
- **Dynamic Computation:** The view is computed dynamically whenever the user performs any query on it.
- **Reflects Changes in Base Table:** Changes made at any point in the view are reflected in the actual base table.

### **Purposes of Views**
1. **Simplify Complex SQL Queries** – Views help in reducing the complexity of queries by encapsulating them in a single entity.
2. **Restrict Access to Sensitive Data** – Views provide a mechanism to restrict user access to certain data, improving security.

### **Example of Creating a View**
```sql
CREATE VIEW EmployeeView AS
SELECT EmployeeID, Name, Department
FROM Employees
WHERE Department = 'IT';
```

This view allows users to retrieve only **EmployeeID, Name, and Department** from the `Employees` table, filtered for the IT department.

### **Using the View**
```sql
SELECT * FROM EmployeeView;
```

### **CRUD Operations on Views**
Views support limited **CRUD** (Create, Read, Update, Delete) operations, depending on whether they are **updatable views** or **non-updatable views**.

#### **1. Creating a View (Create)**
```sql
CREATE VIEW ActiveEmployees AS
SELECT EmployeeID, Name, Status
FROM Employees
WHERE Status = 'Active';
```

#### **2. Reading Data from a View (Read)**
```sql
SELECT * FROM ActiveEmployees;
```

#### **3. Updating Data in a View (Update)**
If the view is based on a **single table** and does not involve joins or aggregate functions, you can update it.
```sql
UPDATE EmployeeView
SET Department = 'HR'
WHERE EmployeeID = 101;
```

This change will also reflect in the base `Employees` table.

#### **4. Deleting Data from a View (Delete)**
If the view is updatable, you can delete records.
```sql
DELETE FROM EmployeeView WHERE EmployeeID = 102;
```

This will remove the record from the base `Employees` table.

### **Limitations of CRUD on Views**
- A view that involves **joins, aggregate functions, subqueries, or DISTINCT** is **not updatable**.
- You **cannot insert** a row into a view if it does not map directly to a row in the base table.
- **Triggers** may be required to support updates on complex views.

### **Conclusion**
Views in SQL are an essential tool for database management, simplifying queries and enhancing security by restricting data access. They provide a flexible and efficient way to work with data without modifying the actual tables. However, CRUD operations on views have limitations depending on the complexity of the view.

