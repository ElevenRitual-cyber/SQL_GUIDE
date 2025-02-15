### **Wildcards in SQL and How They Work**
Wildcards are special characters used in SQL queries with the `LIKE` operator to perform **pattern matching** in text-based searches. They allow you to filter results based on partial values instead of exact matches.

---

## **1. Wildcards in SQL (`LIKE` Operator)**
Wildcards in SQL are mainly used with the `LIKE` operator to match patterns in `VARCHAR`, `TEXT`, or `CHAR` fields.

### **Common Wildcards in SQL**
| Wildcard | Description | Example |
|----------|------------|---------|
| `%` | Matches **zero or more characters** | `WHERE name LIKE 'A%'` → Names starting with 'A' |
| `_` | Matches **exactly one character** | `WHERE name LIKE 'J_hn'` → Matches 'John' or 'Jahn' |
| `[ ]` | Matches **a single character within the brackets** (Only in SQL Server) | `WHERE name LIKE 'Jo[sh]n'` → Matches 'John' or 'Jahn' (SQL Server only) |
| `[^ ]` | Matches **a single character NOT in the brackets** (Only in SQL Server) | `WHERE name LIKE 'Jo[^s]n'` → Matches 'John' but not 'Josn' |
| `-` | Defines a range of characters inside brackets (Only in SQL Server) | `WHERE name LIKE 'Jo[a-z]n'` → Matches 'John', 'Joan', etc. (SQL Server only) |

> **Important:** `[ ]`, `[^ ]`, and `-` work **only in SQL Server**. For MySQL, PostgreSQL, and other databases, use `REGEXP`.

---

### **Examples of `LIKE` Wildcards**
#### **1. Names that start with 'A'**
```sql
SELECT * FROM customers WHERE name LIKE 'A%';
```
- Matches: `Alice`, `Andrew`, `Alex`
- Doesn't match: `Brad`, `Charlie`

#### **2. Names that end with 'n'**
```sql
SELECT * FROM customers WHERE name LIKE '%n';
```
- Matches: `John`, `Ethan`, `Dylan`
- Doesn't match: `Anna`, `Mike`

#### **3. Names that contain 'in' anywhere**
```sql
SELECT * FROM customers WHERE name LIKE '%in%';
```
- Matches: `Martin`, `Linda`, `Kevin`
- Doesn't match: `Alice`, `Bob`

#### **4. Names that have exactly 4 letters and start with 'J'**
```sql
SELECT * FROM customers WHERE name LIKE 'J___';
```
- Matches: `John`, `Jane`
- Doesn't match: `James`, `Jake`

---

## **2. Using Regular Expressions (`REGEXP`) for Advanced Pattern Matching**
For more advanced pattern matching, **`REGEXP`** is used instead of `LIKE`.

### **Common `REGEXP` Patterns**
| Pattern | Description | Example |
|---------|------------|---------|
| `^A` | Starts with 'A' | `WHERE name REGEXP '^A'` |
| `n$` | Ends with 'n' | `WHERE name REGEXP 'n$'` |
| `[aeiou]` | Contains a vowel | `WHERE name REGEXP '[aeiou]'` |
| `[0-9]` | Contains a digit | `WHERE name REGEXP '[0-9]'` |
| `[^a-z]` | Doesn't contain lowercase letters | `WHERE name REGEXP '[^a-z]'` |

#### **Example: Find names starting with "J" and ending with "n"**
```sql
SELECT * FROM customers WHERE name REGEXP '^J.*n$';
```
- Matches: `John`, `Jason`
- Doesn't match: `Jane`, `Jordan`

---

## **Key Differences: `LIKE` vs. `REGEXP`**
| Feature | `LIKE` | `REGEXP` |
|---------|--------|---------|
| **Supports wildcards** | `%`, `_` | `^`, `$`, `[ ]`, `.*`, `[0-9]`, etc. |
| **Can match exact character positions** | No | Yes |
| **Flexible for complex patterns** | No | Yes |
| **Case-sensitive in MySQL?** | No | Yes (default) |

---

## **Conclusion**
- **Use `LIKE`** when searching for simple patterns like "starts with" or "contains".
- **Use `REGEXP`** when you need **advanced pattern matching** like filtering numbers, special characters, or specific character sequences.
- <a href="https://www.geeksforgeeks.org/regular-expressions-in-sql/" target="_blank">For more information</a>

