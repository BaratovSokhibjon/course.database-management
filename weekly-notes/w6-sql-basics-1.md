# Week 6 - Basic SQL - Read This before w5

## 💬 SQL Fundamentals: Speaking to Your Database

> 🎯 SQL (Structured Query Language) is the universal language of databases. Master these fundamentals, and you can talk to any relational database in the world!

---

## 🏛️ The Foundation: Data Definition

### Creating Your Database Structure

SQL uses familiar terms instead of mathematical jargon:

- **📊 Table** = Relation
- **📝 Row** = Tuple  
- **📋 Column** = Attribute

#### Building Blocks

- **Schema** - Collection of tables, constraints, and views
- **Catalog** - Collection of schemas

```sql
CREATE SCHEMA company AUTHORIZATION 'jsmith';
```

- **Base Tables** - Physically stored on disk
- **Views** - Virtual tables computed on-the-fly

---

## 🎨 Data Types: Choosing the Right Format

### Numeric Types

- **`INT`** - Whole numbers
- **`SMALLINT`** - Smaller whole numbers
- **`FLOAT`, `REAL`, `DOUBLE PRECISION`** - Decimal numbers

### Text Types

- **`CHAR(n)`** - Fixed-length string (padded with spaces)
- **`VARCHAR(n)`** - Variable-length string (up to n characters)

> 💡 **When to use which?** Use `CHAR` for fixed codes (like state abbreviations: 'CA'). Use `VARCHAR` for variable text (like names).

### Boolean & Binary

- **`BOOLEAN`** - TRUE, FALSE, or NULL
- **`BIT(n)`** - Fixed-length bit string
- **`BIT VARYING(n)`** - Variable-length bit string

### Date & Time

- **`DATE`** - YYYY-MM-DD
- **`TIME`** - HH:MM:SS
- **`TIMESTAMP`** - Date + Time combined
- **`INTERVAL`** - Duration (e.g., "3 days", "2 hours")

### Custom Types

**Domains** - Named data types with constraints
```sql
CREATE DOMAIN SSN_TYPE AS CHAR(9);
```

**User-Defined Types (UDTs)** - Object-oriented support
```sql
CREATE TYPE AddressType AS (
  Street VARCHAR(50),
  City VARCHAR(30),
  Zip CHAR(5)
);
```

---

## 🛡️ Constraints: The Rules of the Game

Constraints keep your data clean and consistent!

### Primary Keys

```sql
Dnumber INT PRIMARY KEY;
```
> Uniquely identifies each row

### Unique Constraints

```sql
Dname VARCHAR(15) UNIQUE;
```
> No two rows can have the same value (but NULL is allowed)

### Foreign Keys

```sql
FOREIGN KEY (Dno) REFERENCES DEPARTMENT(Dnumber) 
  ON DELETE CASCADE
  ON UPDATE CASCADE;
```

> 🔗 **CASCADE magic:** When you delete a department, all employees in that department are automatically deleted too. Use carefully!

### Check Constraints

```sql
CHECK (Dnumber > 0 AND Dnumber < 21);
CHECK (Salary >= 30000);
```
> Custom rules your data must follow

### Naming Constraints

```sql
CONSTRAINT SalaryCheck CHECK (Salary > 0);
```
> Give constraints names so you can reference them later

---

## 🔍 Basic Queries: SELECT-FROM-WHERE

The holy trinity of SQL queries!

### The Structure

```sql
SELECT <what you want>
FROM <where it lives>
WHERE <conditions it must meet>;
```

> ⭐ **Remember:** SELECT what, FROM where, WHERE conditions. That's it!

### Simple Query Example

**Find the birthday and address of John B. Smith:**
```sql
SELECT Bdate, Address
FROM EMPLOYEE
WHERE Fname = 'John' AND Lname = 'Smith';
```

### Operators You'll Use

- **`=`** - Equal to
- **`<>` or `!=`** - Not equal to
- **`<`, `<=`, `>`, `>=`** - Comparisons
- **`AND`, `OR`, `NOT`** - Logical operators

---

## 🎭 Aliases: Avoiding Confusion

When you need the same table twice, use aliases!

```sql
SELECT E.Fname AS Employee, S.Fname AS Supervisor
FROM EMPLOYEE E, EMPLOYEE S
WHERE E.Super_ssn = S.Ssn;
```
> "Show me employees and their supervisors' names"

**E** and **S** are aliases - different names for the same EMPLOYEE table.

---

## ⭐ Special Selectors

### The Asterisk Wildcard

```sql
SELECT * FROM EMPLOYEE;
```
> "Give me everything!" - Returns all columns

### No WHERE Clause

```sql
SELECT Fname, Lname FROM EMPLOYEE;
```
> Returns ALL rows (no filtering)

---

## 🧩 Set Operations: Combining Results

- **`UNION`** - Combine results, remove duplicates
- **`UNION ALL`** - Combine results, keep duplicates
- **`EXCEPT`** - Results in first query but not second
- **`INTERSECT`** - Results in both queries

```sql
SELECT Fname FROM EMPLOYEE WHERE Dno = 5
UNION
SELECT Fname FROM EMPLOYEE WHERE Salary > 50000;
```

---

## 🔤 String Matching: Finding Patterns

**`LIKE`** operator with wildcards:

- **`%`** - Matches any sequence of characters
- **`_`** - Matches exactly one character

```sql
WHERE Address LIKE '%Houston%';
```
> Find any address containing "Houston"

```sql
WHERE Fname LIKE 'J___';
```
> Find 4-letter names starting with J (John, Jane, Jake)

---

## 🔢 Numeric Operations

### BETWEEN: Range Filtering

```sql
WHERE Salary BETWEEN 40000 AND 60000;
```
> Salaries from $40K to $60K (inclusive)

### Arithmetic Operations

```sql
SELECT Fname, Salary, Salary * 1.1 AS NewSalary
FROM EMPLOYEE;
```
> Calculate 10% raise for everyone

**Operators:** `+`, `-`, `*`, `/`

---

## 📊 Sorting Results: ORDER BY

```sql
SELECT Fname, Lname, Salary
FROM EMPLOYEE
ORDER BY Salary DESC, Lname ASC;
```

- **DESC** - Descending (highest first)
- **ASC** - Ascending (lowest first, default)

> Sort by salary (high to low), then by last name (A to Z) for ties

---

## ✏️ Modifying Data: INSERT, DELETE, UPDATE

### INSERT: Adding New Data

**Single row:**
```sql
INSERT INTO EMPLOYEE (Fname, Lname, Ssn, Salary, Dno)
VALUES ('Jane', 'Doe', '123456789', 55000, 5);
```

**Bulk load from query:**
```sql
INSERT INTO HighEarners
SELECT * FROM EMPLOYEE WHERE Salary > 70000;
```

### DELETE: Removing Data

```sql
DELETE FROM EMPLOYEE WHERE Dno = 5;
```
> Delete all employees in department 5

> ⚠️ **Warning:** DELETE without WHERE deletes EVERYTHING!
> `DELETE FROM EMPLOYEE;` - Empties the entire table!

### UPDATE: Changing Data

**Update one column:**
```sql
UPDATE PROJECT 
SET Plocation = 'Bellaire' 
WHERE Pnumber = 10;
```

**Update multiple columns:**
```sql
UPDATE EMPLOYEE
SET Salary = Salary * 1.1, Dno = 4
WHERE Dno = 5;
```
> Give 10% raise and transfer everyone from dept 5 to dept 4

---

## 🎓 Key Takeaways

> ✨ **SQL is a complete language for:**
> 
> 🏗️ **Data Definition** - CREATE schemas, tables, domains
> 🔍 **Queries** - SELECT-FROM-WHERE pattern
> ✏️ **Data Manipulation** - INSERT, UPDATE, DELETE
> 🛡️ **Constraints** - Keys, checks, foreign keys
> 📊 **Results** - Sorting, filtering, set operations

💡 **Pro tip:** Start with simple SELECT queries to explore your data. Once you understand the structure, move on to JOINs and complex queries. Always test UPDATE and DELETE with a SELECT first to make sure you're targeting the right rows!

### 🔧 SQL Evolution:

- **SQL-86** - The original standard
- **SQL-92 (SQL-2)** - Major expansion
- **SQL:1999 (SQL-3)** - Object-oriented features
- **SQL:2006** - XML support
- **Today** - Constantly evolving with new features!

🎯 **Remember:** SQL is declarative - you say WHAT you want, not HOW to get it. The database figures out the most efficient way!