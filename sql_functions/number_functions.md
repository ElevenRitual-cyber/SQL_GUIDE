### **Generating Random Numbers in SQL**  

SQL provides functions to generate random numbers, which are useful for sampling data, testing, and assigning random values. The method varies depending on the database system.

---

## **1️⃣ MySQL: `RAND()`**
In MySQL, you can use `RAND()` to generate a random decimal between `0` and `1`.

### **✅ Example: Generating a Random Number**
```sql
SELECT RAND(); 
```
🔹 Returns a random decimal between `0` and `1`.  
🔹 Example output: `0.738291`

### **✅ Generating a Random Integer**
To generate a random integer in a range:
```sql
SELECT FLOOR(RAND() * (max - min + 1)) + min;
```
For example, to get a random number between `1` and `100`:
```sql
SELECT FLOOR(RAND() * 100) + 1;
```

### **✅ Selecting a Random Row from a Table**
```sql
SELECT * FROM customers ORDER BY RAND() LIMIT 1;
```
🔹 This fetches a random row from the `customers` table.

---
