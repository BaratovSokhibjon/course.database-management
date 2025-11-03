# Week 5 - More SQL

## 🚀 Advanced SQL: Unlocking Database Superpowers

> ⚡ You've learned basic SQL. Now it's time to level up! Master nested queries, joins, aggregates, views, and triggers - the tools that turn you from a database user into a database wizard.

---

## 🌫️ Dealing with the Unknown: NULL Values

SQL doesn't live in a binary world. It uses **3-valued logic:**

- ✅ **TRUE**
- ❌ **FALSE**  
- ❓ **UNKNOWN** - when NULL values are involved

> 💡 **Pro tip:** Never use `= NULL` or `!= NULL`. Always use `IS NULL` or `IS NOT NULL`.
> 
> Why? Because `NULL = NULL` evaluates to UNKNOWN, not TRUE!

---

## 🪆 Nested Queries: Queries Within Queries

Sometimes one query isn't enough. **Subqueries** let you nest a `SELECT` inside another query's `WHERE` or `FROM` clause.

### Key Operators

**`IN`** - Check if value exists in subquery result
```sql
SELECT Name FROM Employee 
WHERE Dno IN (SELECT Dnumber FROM Department WHERE Dlocation = 'Houston');
```

**`ANY` / `SOME`** - TRUE if condition holds for *at least one* value
```sql
SELECT Name FROM Employee
WHERE Salary > ANY (SELECT Salary FROM Employee WHERE Dno = 5);
```

**`ALL`** - TRUE if condition holds for *every* value
```sql
SELECT Name FROM Employee
WHERE Salary > ALL (SELECT Salary FROM Employee WHERE Dno = 5);
```

💡 **Aliases save the day** when you need to reference the same table multiple times!

### 🔗 Correlated Nested Queries

These are special - the subquery **depends on** the outer query's current tuple.

> 🔄 **Key difference:** Correlated subqueries are evaluated **for each tuple** in the outer query. Non-correlated subqueries run once.

#### Powerful Tools

- **`EXISTS`** - TRUE if subquery returns at least one tuple
- **`NOT EXISTS`** - TRUE if subquery returns zero tuples

```sql
SELECT Name FROM Employee E
WHERE EXISTS (SELECT * FROM Dependent D WHERE D.Essn = E.Ssn);
```
> "Find employees who have at least one dependent"

- **`UNIQUE(Q)`** - TRUE if subquery Q has no duplicate tuples

#### The "For All" Trick

Want to express "for all X, condition Y"? Use **double negation** with `NOT EXISTS`!

> "Find employees who work on ALL projects in department 5"
> 
> Translation: "Find employees for whom there does NOT EXIST a project in dept 5 that they DON'T work on"

---

## 🔀 Joins: Connecting the Dots

Joins combine data from multiple tables. They're the heart of relational databases!

### Join Types

**⚡ INNER JOIN** (default) - Returns only matching tuples from both tables
```sql
SELECT E.Name, D.Dname
FROM Employee E INNER JOIN Department D ON E.Dno = D.Dnumber;
```

**🌿 NATURAL JOIN** - Automatic join on columns with the same name (use carefully!)

**⬅️ LEFT OUTER JOIN** - Keep all tuples from the left table; pad missing right values with NULL
```sql
SELECT E.Name, D.Dname
FROM Employee E LEFT OUTER JOIN Department D ON E.Dno = D.Dnumber;
```
> "Show all employees, even those not assigned to a department"

**➡️ RIGHT OUTER JOIN** - Keep all tuples from the right table

**🔄 FULL OUTER JOIN** - Keep all tuples from both sides

> 💼 **Multiway joins:** You can chain multiple joins together in the `FROM` clause to connect 3, 4, or more tables!

---

## 📊 Aggregate Functions: Crunching Numbers

Make your data tell a story with aggregation!

- **`COUNT(*)`** - Count rows
- **`SUM(col)`** - Add up values
- **`AVG(col)`** - Calculate average
- **`MAX(col)`** - Find maximum
- **`MIN(col)`** - Find minimum

🎯 **Important:** These functions automatically ignore NULL values.

### GROUP BY: Organizing Data

Group tuples by one or more attributes, then aggregate within each group:

```sql
SELECT Dno, COUNT(*), AVG(Salary)
FROM Employee
GROUP BY Dno;
```
> "For each department, how many employees and what's the average salary?"

### HAVING: Filtering Groups

- **WHERE** filters individual tuples *before* grouping
- **HAVING** filters groups *after* aggregation

```sql
SELECT Dno, COUNT(*) AS NumEmployees
FROM Employee
WHERE Salary > 40000
GROUP BY Dno
HAVING COUNT(*) > 5;
```
> "Find departments with more than 5 employees earning over $40,000"

### 🛠️ Bonus Tools

**WITH Clause** - Define temporary named subqueries (like mini-views)
```sql
WITH HighPaid AS (SELECT * FROM Employee WHERE Salary > 80000)
SELECT * FROM HighPaid WHERE Dno = 5;
```

**CASE** - Conditional logic inside queries
```sql
SELECT Name, 
  CASE 
    WHEN Salary > 70000 THEN 'High'
    WHEN Salary > 50000 THEN 'Medium'
    ELSE 'Entry'
  END AS SalaryBand
FROM Employee;
```

---

## 🔁 Recursive Queries: For Hierarchies

Perfect for organizational charts, bill-of-materials, network routing - anything with parent-child relationships!

> 🌳 **Example use case:** Find all employees under a manager, no matter how many levels deep the reporting structure goes.

The query keeps adding tuples recursively until no new rows are found.

---

## ⚙️ Constraints: Enforcing Business Rules

### Assertions

**`CREATE ASSERTION`** - Global constraints beyond simple `CHECK` clauses
```sql
CREATE ASSERTION SalaryConstraint
CHECK (NOT EXISTS (
  SELECT * FROM Employee WHERE Salary > 100000 AND Dno = 1
));
```

### Triggers: Automatic Actions

**Triggers** are the database's way of saying "when X happens, do Y automatically."

**Structure:**
- **Event** - INSERT, UPDATE, or DELETE
- **Condition** - WHEN (optional)
- **Action** - THEN (what to do)

```sql
CREATE TRIGGER LogSalaryChange
AFTER UPDATE OF Salary ON Employee
FOR EACH ROW
WHEN (NEW.Salary > OLD.Salary * 1.5)
BEGIN
  INSERT INTO AuditLog VALUES (OLD.Ssn, OLD.Salary, NEW.Salary, CURRENT_DATE);
END;
```
> "Log any salary increases over 50%"

**Common uses:** Auditing, enforcing complex rules, auto-updates, notifications

---

## 👁️ Views: Virtual Windows Into Your Data

Views are **virtual tables** defined by queries - they don't store data, they compute it on the fly.

```sql
CREATE VIEW Dept5Employees AS
SELECT Name, Ssn, Salary
FROM Employee
WHERE Dno = 5;
```

Now you can query the view just like a table:
```sql
SELECT * FROM Dept5Employees WHERE Salary > 50000;
```

### View Strategies

**📺 Non-Materialized (Query Modification)** - Computed every time you use the view

**💾 Materialized** - Stored temporarily and updated when base tables change
- **Immediate** - Update instantly when data changes
- **Lazy** - Update when view is accessed
- **Periodic** - Update on a schedule

### Views for Security

> 🔒 **Power move:** Create views to restrict what different users can see.
> 
> Give a manager access to `Dept5Employees` view instead of the entire `Employee` table!

**`WITH CHECK OPTION`** - Ensures updates through the view don't violate the view's conditions

---

## 🔧 Schema Modification: Changing the Blueprint

Your database isn't set in stone. Evolve it as needs change!

### Dropping Elements

**`DROP TABLE TableName [CASCADE | RESTRICT]`**
- **CASCADE** - Remove dependent objects too (views, constraints, etc.)
- **RESTRICT** - Fail if dependencies exist (safer!)

### Altering Tables

**Add a column:**
```sql
ALTER TABLE Employee ADD COLUMN BonusRate DECIMAL(3,2);
```

**Drop a column:**
```sql
ALTER TABLE Employee DROP COLUMN BonusRate CASCADE;
```

**Add a constraint:**
```sql
ALTER TABLE Employee ADD CONSTRAINT SalaryCheck CHECK (Salary >= 30000);
```

**Change a column:**
```sql
ALTER TABLE Employee ALTER COLUMN Salary SET DEFAULT 50000;
```

---

## 🎓 Key Takeaways

> ✨ **You've now mastered:**
> 
> 🪆 **Nested & correlated queries** - Query inception!
> 🔀 **Joins** - INNER, LEFT, RIGHT, FULL OUTER
> 📊 **Aggregates & GROUP BY** - Turning data into insights
> ⚙️ **Assertions & Triggers** - Enforcing rules automatically
> 👁️ **Views** - Virtual tables and security
> 🔧 **Schema changes** - ALTER and DROP commands

💡 **Pro tip:** Start simple and build complexity gradually. Test subqueries independently before nesting them. Use views to simplify complex queries you run frequently!

🎯 **Remember:** SQL is a tool for asking questions. The better you understand your data's story, the better questions you can ask.