## **Basic SQL Commands**
Here are some essential SQL commands used for database operations:

---

### **1. Display All Databases**
✔ **Command:**  
   ```sql
   SHOW DATABASES;
   ```
✔ **Description:**  
   - Lists all available databases in the MySQL server.

---

### **2. Select and Use a Specific Database**
✔ **Command:**  
   ```sql
   USE database_name;
   ```
✔ **Description:**  
   - Switches to the specified database for executing queries.

---

### **3. Check the Current Selected Database**
✔ **Command:**  
   ```sql
   SELECT DATABASE();
   ```
✔ **Description:**  
   - Returns the name of the currently selected database.

---

### **4. Show Tables in the Current Database**
✔ **Command:**  
   ```sql
   SHOW TABLES;
   ```
✔ **Description:**  
   - Lists all tables present in the selected database.

---

### **5. Describe Table Structure**
✔ **Command:**  
   ```sql
   DESC table_name;
   ```
✔ **Alternative:**  
   ```sql
   DESCRIBE table_name;
   ```
✔ **Description:**  
   - Shows the structure of the table, including column names, data types, and constraints.

---

### **6. Show Database Structure (Tables and Columns)**
✔ **Command:**  
   ```sql
   SHOW COLUMNS FROM table_name;
   ```
✔ **Description:**  
   - Displays details of all columns in the specified table.

---

### **7. Show Database Creation Query**
✔ **Command:**  
   ```sql
   SHOW CREATE DATABASE database_name;
   ```
✔ **Description:**  
   - Displays the SQL statement used to create the specified database.

---

### **8. Show Table Creation Query**
✔ **Command:**  
   ```sql
   SHOW CREATE TABLE table_name;
   ```
✔ **Description:**  
   - Displays the SQL statement used to create the specified table.

---

### **9. Drop (Delete) a Database**
✔ **Command:**  
   ```sql
   DROP DATABASE database_name;
   ```
✔ **Description:**  
   - Deletes the specified database permanently.

---

### **10. Drop (Delete) a Table**
✔ **Command:**  
   ```sql
   DROP TABLE table_name;
   ```
✔ **Description:**  
   - Deletes the specified table and all its data.

---

These are some fundamental SQL commands to manage databases efficiently. 🚀
