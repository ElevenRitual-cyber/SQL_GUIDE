 Categorized list of **common SQL string functions**:

---

## **1️⃣ Case Conversion Functions** (Changing Uppercase & Lowercase)  
| Function      | Description | Example Output |
|--------------|-------------|---------------|
| `UPPER(str)` | Converts string to uppercase | `UPPER('hello')` → `'HELLO'` |
| `LOWER(str)` | Converts string to lowercase | `LOWER('HELLO')` → `'hello'` |

---

## **2️⃣ String Concatenation Functions** (Joining Strings)  
| Function      | Description | Example Output |
|--------------|-------------|---------------|
| `CONCAT(str1, str2, ...)` | Joins multiple strings together | `CONCAT('Hello', ' ', 'World')` → `'Hello World'` |
| `CONCAT_WS(separator, str1, str2, ...)` | Joins strings with a separator | `CONCAT_WS('-', '2024', '02', '14')` → `'2024-02-14'` |

---

## **3️⃣ Substring Extraction Functions** (Getting a Part of a String)  
| Function      | Description | Example Output |
|--------------|-------------|---------------|
| `SUBSTRING(str, start, length)` | Extracts substring from position `start` | `SUBSTRING('database', 2, 4)` → `'atab'` |
| `MID(str, start, length)` | MySQL alternative to `SUBSTRING()` | `MID('database', 2, 4)` → `'atab'` |
| `LEFT(str, n)` | Returns `n` leftmost characters | `LEFT('database', 4)` → `'data'` |
| `RIGHT(str, n)` | Returns `n` rightmost characters | `RIGHT('database', 3)` → `'ase'` |

---

## **4️⃣ String Length Functions** (Checking String Size)  
| Function      | Description | Example Output |
|--------------|-------------|---------------|
| `LENGTH(str)` | Returns length in bytes (MySQL) | `LENGTH('SQL')` → `3` |
| `CHAR_LENGTH(str)` | Returns length in characters | `CHAR_LENGTH('SQL')` → `3` |

---

## **5️⃣ Searching for Substrings** (Finding Text in a String)  
| Function      | Description | Example Output |
|--------------|-------------|---------------|
| `LOCATE(substr, str)` | Returns position of first occurrence | `LOCATE('a', 'database')` → `2` |
| `POSITION(substr IN str)` | Same as `LOCATE()` | `POSITION('base' IN 'database')` → `5` |
| `INSTR(str, substr)` | Returns position of `substr` in `str` | `INSTR('database', 'base')` → `5` |

---

## **6️⃣ String Modification Functions** (Replacing & Trimming)  
| Function      | Description | Example Output |
|--------------|-------------|---------------|
| `REPLACE(str, old_substr, new_substr)` | Replaces all occurrences of `old_substr` with `new_substr` | `REPLACE('SQL is great', 'great', 'awesome')` → `'SQL is awesome'` |
| `TRIM(str)` | Removes leading & trailing spaces | `TRIM('  SQL  ')` → `'SQL'` |
| `LTRIM(str)` | Removes leading spaces | `LTRIM('  SQL')` → `'SQL'` |
| `RTRIM(str)` | Removes trailing spaces | `RTRIM('SQL  ')` → `'SQL'` |

---

## **7️⃣ Pattern Matching & Wildcard Searching**  
| Function      | Description | Example Output |
|--------------|-------------|---------------|
| `LIKE` | Pattern match with `%` (multiple chars) & `_` (single char) | `LIKE 'A%'` (Starts with 'A') |
| `REGEXP` | Regular expression pattern matching | `REGEXP '^A[b-d]'` (Starts with A followed by b, c, or d) |

---

## **🔥 Quick Summary by Category**
| Category | Functions |
|----------|----------|
| **Case Conversion** | `UPPER()`, `LOWER()` |
| **Concatenation** | `CONCAT()`, `CONCAT_WS()` |
| **Substring Extraction** | `SUBSTRING()`, `MID()`, `LEFT()`, `RIGHT()` |
| **Length Functions** | `LENGTH()`, `CHAR_LENGTH()` |
| **Search Functions** | `LOCATE()`, `POSITION()`, `INSTR()` |
| **Modification & Trimming** | `REPLACE()`, `TRIM()`, `LTRIM()`, `RTRIM()` |
| **Pattern Matching** | `LIKE`, `REGEXP` |


## Note: In sql indexing starts from 1 instead of 0.

## How some functions works?
### **How `CONCAT_WS()` Works Internally in SQL**
The `CONCAT_WS()` function in SQL **concatenates multiple values** into a **single string**, using a **specified separator**. It works as follows:

---

### **🔹 Step-by-Step Working Mechanism**
1. **Takes the separator** as the **first argument**.
2. **Processes each subsequent argument** (string/column values).
3. **Concatenates them** with the given separator.
4. **Ignores `NULL` values** (does not insert extra separators for them).
5. **Returns the final concatenated string**.

---

### **🔹 Example 1: Basic Concatenation**
```sql
SELECT CONCAT_WS('-', '2025', '02', '15');
```
✅ **Output:**  
```
2025-02-15
```
**How it works:**  
- The separator (`'-'`) is applied between `'2025'`, `'02'`, and `'15'`.
- The final result is `"2025-02-15"`.

---

### **🔹 Example 2: Handling `NULL` Values**
```sql
SELECT CONCAT_WS('-', '2025', NULL, '15');
```
✅ **Output:**  
```
2025-15
```
**How it works:**  
- `'2025'` and `'15'` are concatenated.
- The `NULL` value is **ignored** (no extra `'-'` appears).
- The result is `"2025-15"`.

---

### **🔹 Example 3: Using Columns**
Imagine a `customers` table with `firstName` and `lastName` columns:
| firstName | lastName  |
|-----------|----------|
| John      | Doe      |
| Alice     | NULL     |
| Bob       | Smith    |

```sql
SELECT CONCAT_WS(' ', firstName, lastName) AS FullName FROM customers;
```
✅ **Output:**
```
John Doe
Alice
Bob Smith
```
**How it works:**  
- **John + Doe → "John Doe"**
- **Alice + NULL → "Alice"** (NULL is ignored)
- **Bob + Smith → "Bob Smith"**

---

### **🔹 Key Differences Between `CONCAT_WS()` and `CONCAT()`**
| Feature      | `CONCAT_WS()`      | `CONCAT()`  |
|-------------|--------------------|------------|
| Separator   | User-defined        | No separator |
| `NULL` handling | **Ignores NULLs** | Returns NULL |
| Use Case    | **Joins with separators** | **Simple merging** |

✅ **Example Difference**
```sql
SELECT CONCAT_WS('-', 'A', NULL, 'B'); -- Output: "A-B"
SELECT CONCAT('A', NULL, 'B');         -- Output: NULL
```

---

### **🔹 When to Use `CONCAT_WS()`?**
✔️ **When you need a separator** (like `,`, `-`, `|`, etc.).  
✔️ **When handling NULL values gracefully** (without breaking the string).  
✔️ **For formatted outputs** (like dates, full names, addresses).  

