### 1.**AUTO_INCREMENT Constraint in SQL**  

The **AUTO_INCREMENT** constraint is used in SQL to automatically generate a unique **sequential number** for a column whenever a new row is inserted. This is commonly used for **primary key** columns.

---

### **How It Works**
1. When a table has an **AUTO_INCREMENT** column, the database automatically assigns an increasing number to each new row.
2. The numbering starts at **1** (by default) and increases by **1** with each new insertion.
3. If a row is deleted, the number is **not reused** (unless manually reset).
4. If the column reaches the maximum limit, you need to reset or alter it manually.
5. The sql will take the largest value from column and increment it by 1.

---

### **Syntax in MySQL**
```sql
CREATE TABLE Employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50)
);
```
- Here, the `id` column is **automatically assigned** a unique number starting from **1**.
- Each time a new row is inserted, the **next available number** is assigned.

---

### **Inserting Data**
```sql
INSERT INTO Employees (name, department) VALUES ('Alice', 'HR');
INSERT INTO Employees (name, department) VALUES ('Bob', 'IT');
```
**Result:**
| id | name  | department |
|----|-------|-----------|
| 1  | Alice | HR        |
| 2  | Bob   | IT        |

- The `id` column values are **automatically generated**.

---

### **Retrieving the Last Inserted ID**
After inserting a record, you can retrieve the **last generated AUTO_INCREMENT value** using:
```sql
SELECT LAST_INSERT_ID();
```

---

### **Manually Setting the Starting Value**
You can **change** the starting value of `AUTO_INCREMENT` using:
```sql
ALTER TABLE Employees AUTO_INCREMENT = 100;
```
Now, the next inserted row will have `id = 100`.

---

### **Skipping a Value**
If an `AUTO_INCREMENT` value is manually set:
```sql
INSERT INTO Employees (id, name, department) VALUES (10, 'Charlie', 'Finance');
```
- The next auto-generated value will be **11** (not 3).

---

### **Deleting Rows and AUTO_INCREMENT Behavior**
If you delete the last inserted row, the `AUTO_INCREMENT` value **does not reset**:
```sql
DELETE FROM Employees WHERE id = 2;
INSERT INTO Employees (name, department) VALUES ('David', 'Marketing');
```
- `David` will get **id = 3**, not 2.

---

### **Reset AUTO_INCREMENT**
To reset the sequence:
```sql
TRUNCATE TABLE Employees;
```
- This **deletes all rows** and resets `AUTO_INCREMENT` to **1**.

Alternatively, you can manually set it:
```sql
ALTER TABLE Employees AUTO_INCREMENT = 1;
```

---

### **Key Points**
✅ Used for **unique IDs** in tables.  
✅ Works with **integer (INT, BIGINT, etc.)** columns.  
✅ Increments **automatically** for new rows.  
✅ **Skipping or deleting IDs does not affect the sequence**.  
✅ **Cannot have AUTO_INCREMENT without a key (usually PRIMARY KEY)**.  

---

### **Conclusion**
The `AUTO_INCREMENT` constraint is essential for handling **unique identifiers** in SQL tables, especially for **primary keys**. It simplifies inserting records without manually assigning unique IDs. 🚀






### 2.**NOT NULL Constraint in SQL**  

The **NOT NULL** constraint in SQL ensures that a column **cannot have NULL values**. This means that every row **must** have a valid (non-null) value for that column.

---

### **How It Works**
1. If a column has a **NOT NULL** constraint, it **must** be given a value when inserting a new row.
2. If an attempt is made to insert a `NULL` value, the database will **throw an error**.
3. It ensures **data integrity** by preventing missing values in important fields.

---

### **Syntax**
```sql
CREATE TABLE Employees (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(50) NOT NULL
);
```
- The `name` and `department` columns **must always** have values.
- The `id` column is **not explicitly** `NOT NULL` but is implicitly `NOT NULL` due to the `PRIMARY KEY`.

---

### **Inserting Data**
✅ **Valid Insert:**
```sql
INSERT INTO Employees (id, name, department) VALUES (1, 'Alice', 'HR');
```
⛔ **Invalid Insert (NULL value in `name` column):**
```sql
INSERT INTO Employees (id, name, department) VALUES (2, NULL, 'IT');
```
❌ **Error:** `Column 'name' cannot be null`

---

### **Altering a Column to Add NOT NULL**
If a column **was not originally** defined as `NOT NULL`, you can modify it:
```sql
ALTER TABLE Employees MODIFY name VARCHAR(100) NOT NULL;
```
⚠️ **If there are existing NULL values in the column, this will fail!**  
Ensure all values are non-null before applying the constraint:
```sql
UPDATE Employees SET name = 'Unknown' WHERE name IS NULL;
```

---

### **Removing the NOT NULL Constraint**
If you need to allow NULL values:
```sql
ALTER TABLE Employees MODIFY name VARCHAR(100) NULL;
```

---

### **Key Points**
✅ **Ensures mandatory values** for a column.  
✅ **Prevents NULL values** to maintain data integrity.  
✅ Can be applied to any **column type** (`INT`, `VARCHAR`, `DATE`, etc.).  
✅ **Can be added later** using `ALTER TABLE`, but only if no NULL values exist.  

---

### **Conclusion**
The **NOT NULL** constraint is crucial for ensuring that critical data fields **always** have values, preventing incomplete or inconsistent data in a database. 🚀


### 3.**UNIQUE Constraint in SQL**  

The **UNIQUE** constraint ensures that all values in a column (or a combination of columns) are **distinct** (i.e., no duplicate values are allowed).

---

### **How It Works**  
1. The **UNIQUE** constraint **prevents duplicate values** in the specified column.  
2. It can be applied to **one or multiple columns**.  
3. Unlike `PRIMARY KEY`, a table **can have multiple UNIQUE columns**.  
4. Unlike `PRIMARY KEY`, a **UNIQUE column can contain NULL values**, but only **one NULL** per column (in most databases).  

---

### **Syntax**  
#### **Defining UNIQUE on a Single Column**
```sql
CREATE TABLE Employees (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(20) UNIQUE
);
```
- The `email` and `phone` columns **must have unique values**.  
- Multiple employees **cannot share the same email or phone number**.  
- The `id` column is `PRIMARY KEY`, which is also **implicitly unique**.

---

#### **Defining UNIQUE on Multiple Columns**
```sql
CREATE TABLE Employees (
    id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    department VARCHAR(50),
    UNIQUE (first_name, last_name, department)
);
```
- Here, the **combination** of `first_name`, `last_name`, and `department` must be unique.  
- Two employees can have the same `first_name` and `last_name`, but not in the **same department**.

---

### **Inserting Data**
✅ **Valid Insert:**
```sql
INSERT INTO Employees (id, email, phone) VALUES (1, 'alice@example.com', '1234567890');
```

⛔ **Invalid Insert (Duplicate email):**
```sql
INSERT INTO Employees (id, email, phone) VALUES (2, 'alice@example.com', '9876543210');
```
❌ **Error:** `Duplicate entry 'alice@example.com' for key 'email'`

---

### **Altering an Existing Table to Add UNIQUE**
If you forgot to add `UNIQUE` during table creation, you can modify it later:
```sql
ALTER TABLE Employees ADD CONSTRAINT unique_email UNIQUE (email);
```
or  
```sql
ALTER TABLE Employees ADD UNIQUE (email);
```

---

### **Removing a UNIQUE Constraint**
To remove a `UNIQUE` constraint:
```sql
ALTER TABLE Employees DROP INDEX unique_email;
```
or (depending on the database):
```sql
ALTER TABLE Employees DROP CONSTRAINT unique_email;
```

---

### **Key Points**
✅ Ensures **no duplicate values** in a column.  
✅ A table **can have multiple UNIQUE constraints**.  
✅ Allows **one NULL value** (depending on the database).  
✅ Can be applied to **single or multiple columns**.  
✅ **Not the same as PRIMARY KEY** (which enforces uniqueness and NOT NULL together).  

---

### **Conclusion**
The `UNIQUE` constraint is essential for **data integrity**, preventing duplicate records while allowing flexibility for multiple unique attributes within a table. 🚀

### 4.**PRIMARY KEY in SQL**  

A **PRIMARY KEY** is a constraint in SQL that uniquely identifies each record in a table. It ensures that the column (or combination of columns) used as the primary key is **unique** and **not null**.

---

### **Key Characteristics of a PRIMARY KEY**  
1. **Uniqueness** – No two rows can have the same primary key value.  
2. **Not Null** – A primary key column **cannot** contain NULL values.  
3. **Only One Per Table** – Each table **can have only one primary key**, but it can consist of **multiple columns** (composite key).  
4. **Auto-Increment** – Often, primary keys use `AUTO_INCREMENT` to generate unique values automatically.  

---

### **Syntax**
#### **Defining a PRIMARY KEY on a Single Column**
```sql
CREATE TABLE Employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50)
);
```
- The `id` column is the **primary key**, meaning every row must have a **unique, non-null** value in `id`.

---

#### **Using PRIMARY KEY with AUTO_INCREMENT**
```sql
CREATE TABLE Employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50)
);
```
- `AUTO_INCREMENT` ensures each new row gets the **next sequential value** automatically.

---

#### **Defining a PRIMARY KEY on Multiple Columns (Composite Key)**
```sql
CREATE TABLE Orders (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);
```
- This **composite key** ensures that each combination of `order_id` and `product_id` is unique.

---

### **Inserting Data**
✅ **Valid Insert:**
```sql
INSERT INTO Employees (id, name, department) VALUES (1, 'Alice', 'HR');
INSERT INTO Employees (id, name, department) VALUES (2, 'Bob', 'IT');
```

⛔ **Invalid Insert (Duplicate `id` value):**
```sql
INSERT INTO Employees (id, name, department) VALUES (1, 'Charlie', 'Finance');
```
❌ **Error:** `Duplicate entry '1' for key 'PRIMARY'`

⛔ **Invalid Insert (NULL value in `id` column):**
```sql
INSERT INTO Employees (id, name, department) VALUES (NULL, 'David', 'Marketing');
```
❌ **Error:** `Column 'id' cannot be null`

---

### **Altering a Table to Add a PRIMARY KEY**
If a table was created without a primary key, you can **add it later**:
```sql
ALTER TABLE Employees ADD PRIMARY KEY (id);
```

---

### **Removing a PRIMARY KEY**
To remove a primary key from a table:
```sql
ALTER TABLE Employees DROP PRIMARY KEY;
```
⚠ **Note:** If the primary key is part of a composite key, you must redefine it explicitly.

---

### **PRIMARY KEY vs. UNIQUE Constraint**
| Feature            | PRIMARY KEY       | UNIQUE Constraint |
|--------------------|------------------|------------------|
| Ensures Uniqueness | ✅ Yes             | ✅ Yes             |
| Allows NULLs      | ❌ No (Not Null)   | ✅ Yes (one NULL allowed) |
| Number Per Table  | 1 per table       | Multiple allowed |
| Auto-Increment    | ✅ Commonly used   | ❌ Not automatic |

---

### **Key Points**
✅ **Uniquely identifies each row** in a table.  
✅ **Cannot be NULL**.  
✅ **Only one PRIMARY KEY** per table (can be a composite key).  
✅ Commonly used with **AUTO_INCREMENT**.  
✅ Cannot be modified unless explicitly dropped and recreated.  

---

### **Conclusion**
A **PRIMARY KEY** is essential for **data integrity and efficient lookups** in relational databases. It ensures that **every record is uniquely identifiable**, preventing duplicate or missing values. 🚀

### 5.**Foreign Key Constraint in SQL** 🔑  

A **Foreign Key** is a constraint in SQL that **establishes a relationship** between two tables. It ensures that the values in a column of one table **must exist** in a column of another table, maintaining **referential integrity**.  
**In a foreign key relationship, the Foreign Key is the column in the child table that references the Primary Key of the parent table.**

---

### **🔹 Key Points of Foreign Key Constraint**
✅ **Links Two Tables** – A column in one table references a **Primary Key** in another table.  
✅ **Ensures Data Integrity** – Prevents orphan records by ensuring referenced values exist.  
✅ **Cascading Operations** – Supports `ON DELETE CASCADE` and `ON UPDATE CASCADE`.  

---

### **🔹 Example: Foreign Key in SQL**
#### **1️⃣ Creating Tables with a Foreign Key**
```sql
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);
```
📌 *Here, `Orders.customer_id` must exist in `Customers.customer_id`.*  
📌In this case:

Customers.customer_id → Primary Key (in the parent table)
Orders.customer_id → Foreign Key (in the child table, referencing Customers.customer_id)
The Foreign Key (customer_id in Orders) ensures that every value it holds must exist in the Customers.customer_id column.

---

### **🔹 Foreign Key with Cascade Options**
#### **2️⃣ ON DELETE CASCADE**  
Automatically deletes related rows in the child table when the referenced row in the parent table is deleted.
```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON DELETE CASCADE
);
```

#### **3️⃣ ON UPDATE CASCADE**  
Updates child table records when the referenced key in the parent table changes.
```sql
FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON UPDATE CASCADE
```

---

### **🔹 Inserting & Violating Foreign Key**
```sql
INSERT INTO Customers VALUES (1, 'Alice');
INSERT INTO Orders VALUES (101, 1, '2024-02-19');  -- ✅ Works

INSERT INTO Orders VALUES (102, 5, '2024-02-19');  -- ❌ ERROR! No customer_id = 5
```
📌 *SQL rejects inserting `customer_id = 5` since it does not exist in `Customers`.*

---

### **🔹 Why Use Foreign Keys?**
✔ Prevents **inconsistent** data  
✔ Ensures **referential integrity**  
✔ Enables **structured** relationships  

#### ### **What is Referential Integrity?** 🔗  

**Referential Integrity** is a **database concept** that ensures relationships between tables remain **consistent** by enforcing rules on **foreign keys**. It prevents **orphaned records** and maintains **data accuracy**.  

---

### **🔹 How Referential Integrity Works**  
1️⃣ A **Foreign Key** in a child table must refer to a **valid** Primary Key in the parent table.  
2️⃣ If a referenced row is deleted or updated, rules like `CASCADE`, `SET NULL`, or `RESTRICT` define what happens.  
3️⃣ Prevents inserting invalid foreign key values that do not exist in the parent table.  

---

### **🔹 Example: Maintaining Referential Integrity**
```sql
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON DELETE CASCADE
);
```

#### ✅ **Valid Operations**  
```sql
INSERT INTO Customers VALUES (1, 'Alice');
INSERT INTO Orders VALUES (101, 1, '2024-02-19');  -- ✅ Allowed (customer_id 1 exists)
```

#### ❌ **Invalid Operations (Prevented by Referential Integrity)**
```sql
INSERT INTO Orders VALUES (102, 5, '2024-02-19');  -- ❌ ERROR! (No customer_id = 5)

DELETE FROM Customers WHERE customer_id = 1;  -- ✅ Deletes customer & their orders (CASCADE)
```

---

### **🔹 Why Is Referential Integrity Important?**  
✔ Prevents **orphan records** (e.g., an order referencing a non-existent customer)  
✔ Ensures **data consistency** across related tables  
✔ Supports **data accuracy** by preventing invalid references  

### **Cascading Rules in SQL** 🔄  

Cascading rules define how **changes in the parent table** affect related rows in the **child table** when using **Foreign Keys**. These rules ensure **referential integrity** by handling `DELETE` and `UPDATE` operations.

---

### **🔹 Types of Cascading Rules**  

#### **1️⃣ ON DELETE CASCADE & ON UPDATE CASCADE** 🔥  
📌 **Effect:** When a row in the parent table is **deleted** or **updated**, all matching rows in the child table are also **deleted** or **updated** automatically.  

```sql
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY
);

CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON DELETE CASCADE ON UPDATE CASCADE
);
```
✅ **Example:**  
```sql
DELETE FROM Customers WHERE customer_id = 1;  -- Deletes all orders with customer_id = 1
UPDATE Customers SET customer_id = 5 WHERE customer_id = 1;  -- Updates customer_id in Orders too
```

---

#### **2️⃣ ON DELETE SET NULL & ON UPDATE SET NULL** ⚠️  
📌 **Effect:** When a parent record is **deleted** or **updated**, the foreign key column in the child table is set to `NULL`.  
⚠️ Requires the foreign key column in the child table to allow `NULL` values.

```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON DELETE SET NULL
);
```
✅ **Example:**  
```sql
DELETE FROM Customers WHERE customer_id = 1;  -- Orders with customer_id = 1 now have NULL
```

---

#### **3️⃣ ON DELETE RESTRICT & ON UPDATE RESTRICT** 🚫  
📌 **Effect:** Prevents deletion or updating of the parent record **if child records exist**.  
💡 This is the default behavior if no cascading rule is specified.

```sql
FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON DELETE RESTRICT
```
❌ **Example:**  
```sql
DELETE FROM Customers WHERE customer_id = 1;  -- ERROR! Orders exist with this customer_id
```

---

#### **4️⃣ ON DELETE NO ACTION & ON UPDATE NO ACTION** ⏸  
📌 **Effect:** Similar to `RESTRICT`, but enforcement is deferred until the end of the transaction.

```sql
FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON DELETE NO ACTION
```
🚫 **Example:**  
```sql
DELETE FROM Customers WHERE customer_id = 1;  -- ❌ Error if referenced in Orders
```

---

### **🔹 Summary Table**
| Rule                 | DELETE Effect                          | UPDATE Effect                          |
|----------------------|--------------------------------------|--------------------------------------|
| **CASCADE**         | Deletes matching rows in the child   | Updates matching foreign keys       |
| **SET NULL**        | Sets foreign key to `NULL`           | Sets foreign key to `NULL`          |
| **RESTRICT**        | Prevents deletion if referenced      | Prevents update if referenced      |
| **NO ACTION**       | Like `RESTRICT`, but enforced later  | Like `RESTRICT`, but deferred       |

---

### **🔹 Choosing the Right Rule**
✔ Use `CASCADE` when child data **must be removed** with the parent.  
✔ Use `SET NULL` if child records should **remain but lose reference**.  
✔ Use `RESTRICT` or `NO ACTION` if deletion or update **should not be allowed**.  

