SQL provides several **date and time** data types, which vary based on the database system. Below is a comprehensive list of **date and time data types** along with their **formats and ranges**.

---

## **1. SQL Date and Time Data Types**

### **1.1 MySQL Date and Time Data Types**
| **Data Type**  | **Description** | **Format Example** | **Storage Size** | **Range** |
|---------------|----------------|------------------|--------------|----------|
| **DATE** | Stores only the date (YYYY-MM-DD) | `'2025-02-15'` | **3 bytes** | `1000-01-01` to `9999-12-31` |
| **DATETIME** | Stores date and time (YYYY-MM-DD HH:MI:SS) | `'2025-02-15 14:30:00'` | **8 bytes** | `1000-01-01 00:00:00` to `9999-12-31 23:59:59` |
| **TIMESTAMP** | Stores date and time, automatically updates (YYYY-MM-DD HH:MI:SS) | `'2025-02-15 14:30:00'` | **4 bytes** | `1970-01-01 00:00:01` to `2038-01-19 03:14:07` |
| **TIME** | Stores only time (HH:MI:SS) | `'14:30:00'` | **3 bytes** | `-838:59:59` to `838:59:59` |
| **YEAR** | Stores only the year (YYYY) | `'2025'` | **1 byte** | `1901` to `2155` |

---


## **2. Explanation of Each Data Type**
### **2.1 DATE**
- Stores only **date** in `YYYY-MM-DD` format.
- No time component.
- Example:
  ```sql
  CREATE TABLE Events (
      id INT,
      event_date DATE
  );
  INSERT INTO Events VALUES (1, '2025-02-15');
  ```

### **2.2 DATETIME**
- Stores **both date and time**.
- Useful for recording timestamps for events.
- Example:
  ```sql
  CREATE TABLE Users (
      id INT,
      registered_at DATETIME
  );
  INSERT INTO Users VALUES (1, '2025-02-15 14:30:00');
  ```

### **2.3 TIMESTAMP**
- Similar to `DATETIME`, but usually **automatically updates** when a record is modified.
- Stored in **UTC** (Universal Coordinated Time).
- Example:
  ```sql
  CREATE TABLE Orders (
      id INT,
      order_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```

### **2.4 TIME**
- Stores only **time (HH:MI:SS)**.
- Used for tracking working hours or schedules.
- Example:
  ```sql
  CREATE TABLE WorkShifts (
      id INT,
      start_time TIME
  );
  INSERT INTO WorkShifts VALUES (1, '09:00:00');
  ```

### **2.5 YEAR**
- Stores **only the year** (4-digit format).
- Useful for storing years like manufacturing years, graduation years.
- Example:
  ```sql
  CREATE TABLE Vehicles (
      id INT,
      manufacture_year YEAR
  );
  INSERT INTO Vehicles VALUES (1, 2023);
  ```


## **3. Choosing the Right Data Type**
| **Use Case** | **Recommended Data Type** |
|-------------|------------------------|
| Store only date (without time) | `DATE` |
| Store both date and time | `DATETIME` / `TIMESTAMP` |
| Track when a record was created/updated | `TIMESTAMP` (with `DEFAULT CURRENT_TIMESTAMP`) |
| Store only time (without date) | `TIME` |
| Store only the year | `YEAR` |
| Work with time zones | `TIMESTAMP WITH TIME ZONE` / `DATETIMEOFFSET` |

##  Datetime  V/S Timestamp
### **Difference Between `DATETIME` and `TIMESTAMP` in SQL**

Both `DATETIME` and `TIMESTAMP` store **date and time values**, but they differ in **storage, range, behavior, and use cases**.

---

## **1. Key Differences Between `DATETIME` and `TIMESTAMP`**
| **Feature** | **DATETIME** | **TIMESTAMP** |
|------------|-------------|--------------|
| **Storage** | Uses **8 bytes** | Uses **4 bytes** (MySQL) |
| **Range** | `1000-01-01 00:00:00` to `9999-12-31 23:59:59` | `1970-01-01 00:00:01 UTC` to `2038-01-19 03:14:07 UTC` |
| **Time Zone Handling** | **Does not** convert based on time zones | Stored in **UTC** and converted to the session’s time zone |
| **Automatic Updates** | **Does not** update automatically | Can **auto-update** when the row is modified |
| **Default Value** | No automatic default (`NULL` if not provided) | Can use `DEFAULT CURRENT_TIMESTAMP` |
| **Best Used For** | Storing fixed date-time values like birthdates, historical records | Storing events, logs, last update timestamps |

---

## **2. When to Use `DATETIME` vs. `TIMESTAMP`**

| **Scenario** | **Use `DATETIME`** | **Use `TIMESTAMP`** |
|-------------|-----------------|-----------------|
| **Historical Events** (e.g., birthdates, invoice creation) | ✅ | ❌ |
| **Logging System Events** (e.g., user login, record update) | ❌ | ✅ |
| **Time Zone Sensitive Data** | ❌ | ✅ |
| **Data Retention Beyond 2038** | ✅ | ❌ |
| **Auto-Updating Last Modified Time** | ❌ | ✅ |
| **Fixed Date & Time (does not change)** | ✅ | ❌ |

---

## **3. Examples**

### **Using `DATETIME`**
```sql
CREATE TABLE Users (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    registration_date DATETIME
);

INSERT INTO Users VALUES (1, 'John Doe', '2025-02-15 14:30:00');
SELECT * FROM Users;
```
✅ **Best for: Fixed timestamps like birth dates, invoice dates, etc.**

---

### **Using `TIMESTAMP`**
```sql
CREATE TABLE Orders (
    id INT PRIMARY KEY,
    order_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

INSERT INTO Orders VALUES (1, DEFAULT);
SELECT * FROM Orders;
```
✅ **Best for: Logging events, automatic updates on modification.**

---

## **4. Summary**
- Use **`DATETIME`** if you **don't want automatic updates** and **don't care about time zones**.
- Use **`TIMESTAMP`** if you need **automatic updates** or need to **store time zone-aware data**.

# 2. **Q: What is the difference between using `DATETIME DEFAULT NOW()` and `TIMESTAMP DEFAULT CURRENT_TIMESTAMP` in MySQL? When should I use each?**  

#### **1️⃣ `DATETIME DEFAULT NOW()`**  
```sql
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    timings DATETIME DEFAULT NOW()
);
```
- ✅ Stores a **fixed** timestamp (`YYYY-MM-DD HH:MM:SS`).
- ✅ **Does NOT update automatically** when the row is modified.
- ✅ **No time zone conversion**; it stores the exact value inserted.
- ❌ **Larger storage size** (8 bytes in MySQL).

---

#### **2️⃣ `TIMESTAMP DEFAULT CURRENT_TIMESTAMP`**  
```sql
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    timings TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
- ✅ **Automatically inserts the current timestamp** when a new row is created.
- ✅ **Can auto-update on row modification** if defined as:
  ```sql
  timings TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
  ```
- ✅ **Time zone aware**; stored values can adjust based on session time zone settings.
- ✅ **Smaller storage size** (4 bytes in MySQL).
- ❌ **Limited date range** (`1970-01-01` to `2038-01-19` in MySQL).

---

### **📌 Key Differences:**
| Feature         | `DATETIME` | `TIMESTAMP` |
|---------------|------------|-------------|
| **Storage Size** | 8 bytes | 4 bytes |
| **Time Zone Aware** | ❌ No | ✅ Yes |
| **Auto-Update on Row Change** | ❌ No | ✅ Yes (if defined with `ON UPDATE CURRENT_TIMESTAMP`) |
| **Date Range** | `1000-01-01` to `9999-12-31` | `1970-01-01` to `2038-01-19` |
| **Best Use Case** | Storing fixed timestamps (e.g., order creation time) | Logging system events, tracking last modified times |


### **SQL Date Functions**  

SQL provides various **date functions** to manipulate and retrieve date and time values. Here’s a list of commonly used date functions across different databases like MySQL, SQL Server, PostgreSQL, and Oracle.

---

### **1. Current Date & Time Functions**  
These functions return the current system date and time.

| Function | Description | Supported Databases |
|----------|------------|--------------------|
| `CURRENT_DATE` | Returns the current date without time. | MySQL, PostgreSQL, SQL Server, Oracle |
| `CURRENT_TIME` | Returns the current time without date. | MySQL, PostgreSQL, SQL Server, Oracle |
| `CURRENT_TIMESTAMP` | Returns the current date and time (timestamp). | MySQL, PostgreSQL, SQL Server, Oracle |
| `NOW()` | Returns the current date and time. | MySQL, PostgreSQL |
| `SYSDATE` | Returns the system’s current date and time. | Oracle, MySQL |
| `GETDATE()` | Returns the current date and time. | SQL Server |
| `GETUTCDATE()` | Returns the current UTC date and time. | SQL Server |

---

### **2. Extracting Date Parts**  
These functions retrieve specific parts of a date.

| Function | Description | Supported Databases |
|----------|------------|--------------------|
| `YEAR(date)` | Extracts the year from a date. | MySQL, SQL Server |
| `MONTH(date)` | Extracts the month from a date. | MySQL, SQL Server |
| `DAY(date)` | Extracts the day from a date. | MySQL, SQL Server |
| `HOUR(time)` | Extracts the hour from a time. | MySQL |
| `MINUTE(time)` | Extracts the minute from a time. | MySQL |
| `SECOND(time)` | Extracts the second from a time. | MySQL |
| `EXTRACT(part FROM date)` | Extracts a specific part (year, month, day, etc.). | PostgreSQL, MySQL |
| `DATEPART(part, date)` | Extracts a specific part from a date. | SQL Server |

**Example:**  
```sql
SELECT YEAR(NOW()), MONTH(NOW()), DAY(NOW()); -- MySQL
SELECT EXTRACT(YEAR FROM CURRENT_DATE); -- PostgreSQL
```

---

### **3. Date Arithmetic (Add & Subtract)**  
These functions add or subtract intervals from a date.

| Function | Description | Supported Databases |
|----------|------------|--------------------|
| `DATE_ADD(date, INTERVAL value unit)` | Adds a specific time interval to a date. | MySQL |
| `DATE_SUB(date, INTERVAL value unit)` | Subtracts a specific time interval from a date. | MySQL |
| `DATEADD(part, value, date)` | Adds a specific time unit to a date. | SQL Server |
| `DATESUB(part, value, date)` | Subtracts a specific time unit from a date. | SQL Server |
| `ADD_MONTHS(date, n)` | Adds `n` months to a date. | Oracle, PostgreSQL |

**Example:**  
```sql
SELECT DATE_ADD('2024-02-15', INTERVAL 7 DAY); -- MySQL
SELECT DATEADD(DAY, 7, '2024-02-15'); -- SQL Server
SELECT ADD_MONTHS(SYSDATE, 2) FROM dual; -- Oracle
```

---

### **4. Date Difference Functions**  
These functions return the difference between two dates.

| Function | Description | Supported Databases |
|----------|------------|--------------------|
| `DATEDIFF(date1, date2)` | Returns the difference in days. | MySQL, SQL Server |
| `TIMESTAMPDIFF(unit, date1, date2)` | Returns the difference between two timestamps. | MySQL |
| `AGE(date1, date2)` | Returns the age difference between two dates. | PostgreSQL |

**Example:**  
```sql
SELECT DATEDIFF('2024-12-31', '2024-02-15'); -- MySQL, SQL Server
SELECT AGE('2024-12-31', '2024-02-15'); -- PostgreSQL
```

---

### **5. Formatting Dates**  
These functions format a date in a specific pattern.

| Function | Description | Supported Databases |
|----------|------------|--------------------|
| `DATE_FORMAT(date, format)` | Formats a date with a specific pattern. | MySQL |
| `TO_CHAR(date, format)` | Converts a date to a formatted string. | Oracle, PostgreSQL |
| `FORMAT(date, format)` | Formats a date in SQL Server. | SQL Server |

**Example:**  
```sql
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s'); -- MySQL
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS') FROM dual; -- Oracle
```

---

### **6. Miscellaneous Date Functions**  

| Function | Description | Supported Databases |
|----------|------------|--------------------|
| `LAST_DAY(date)` | Returns the last day of the month. | Oracle, MySQL |
| `DAYNAME(date)` | Returns the name of the weekday. | MySQL |
| `DAYOFWEEK(date)` | Returns the day of the week (1-7). | MySQL |
| `WEEK(date)` | Returns the week number of the year. | MySQL |
| `EOMONTH(date)` | Returns the last day of the month. | SQL Server |

**Example:**  
```sql
SELECT LAST_DAY(NOW()); -- MySQL
SELECT EOMONTH(GETDATE()); -- SQL Server
```

---

### **Example Queries**
#### **Get the Current Date and Time**
```sql
SELECT NOW(); -- MySQL, PostgreSQL
SELECT SYSDATE; -- Oracle
SELECT GETDATE(); -- SQL Server
```

#### **Find the Difference Between Two Dates**
```sql
SELECT DATEDIFF('2024-12-31', '2024-02-15'); -- MySQL, SQL Server
SELECT AGE('2024-12-31', '2024-02-15'); -- PostgreSQL
```

#### **Extracting Year, Month, and Day**
```sql
SELECT YEAR(NOW()), MONTH(NOW()), DAY(NOW()); -- MySQL
SELECT EXTRACT(YEAR FROM CURRENT_DATE); -- PostgreSQL
```
