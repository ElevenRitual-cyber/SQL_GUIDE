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
