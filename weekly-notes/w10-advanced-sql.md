# Week 10 - Advanced SQL

## 🔗 Accessing SQL from a Programming Language

Ever wondered how real-world applications actually talk to databases? It's not magic—it's through specialized APIs that bridge your code and the database server.

**The Goal:** Enable your application to connect, send SQL commands, and process results seamlessly.

**Popular APIs you'll encounter:**

- **ODBC** — The veteran (C/C++/C#/VB)
- [**ADO.NET**](http://ADO.NET) — Built on top of ODBC for .NET ecosystems
- **JDBC** — Java's database champion
- **Embedded SQL** — Classic approach for host languages

---

## 🌐 ODBC (Open Database Connectivity)

Think of ODBC as the universal translator for databases. This C-based API lets you connect to almost any DBMS out there.

**The typical workflow:**

1. Allocate environment & connection resources
2. Establish connection
3. Prepare and execute your statements
4. Fetch results row by row
5. Clean up and close

Perfect for desktop applications, GUI tools, and even Excel spreadsheets that need database access!

---

## ☕ JDBC (Java Database Connectivity)

JDBC brings database connectivity to the Java world with an elegant object-oriented approach.

**The Pattern:** `Connection` → `Statement` → `ResultSet` → Process → Close

### Modern Java Example (The Clean Way)

```java
try (Connection conn = DriverManager.getConnection(url, user, pass);
     Statement stmt = conn.createStatement()) {
  ResultSet rs = stmt.executeQuery("SELECT dept_name, AVG(salary) FROM instructor GROUP BY dept_name");
  while ([rs.next](http://rs.next)()) System.out.println(rs.getString("dept_name") + " " + rs.getFloat(2));
}
```

The `try-with-resources` statement handles cleanup automatically—no memory leaks, no forgotten closes!

**Legacy approach:** You might see older code using `Class.forName(...)` to load drivers and explicit `close()` calls everywhere.

**Pro tip:** Access columns by name `rs.getString("dept_name")` for readability, or by index `rs.getFloat(2)` for performance. Always check for SQL NULLs using `rs.wasNull()` after retrieving values.

---

## 🛡️ Prepared Statements & SQL Injection

<aside>
⚠️

**Critical Security Practice**

Never, ever concatenate user input directly into SQL strings! That's like leaving your database's front door wide open.

</aside>

### The Safe Way: Prepared Statements

```java
PreparedStatement p = conn.prepareStatement("INSERT INTO instructor VALUES (?, ?, ?, ?)");
p.setString(1, "88877"); 
p.setString(2, "Perry"); 
p.setString(3, "Finance"); 
p.setInt(4, 125000);
p.executeUpdate();
```

**Why prepared statements are your best friend:**

- **Security:** Prevents SQL injection attacks
- **Performance:** Precompiled query plans mean faster execution
- **Clarity:** Separates data from logic

**The dangerous alternative** (DON'T DO THIS):

```java
"SELECT * FROM users WHERE name='" + userName + "'"  // 💀 Vulnerable!
```

An attacker could set `userName` to `"'; DROP TABLE users; --"` and destroy your data!

---

## 🔍 Metadata Features

Sometimes you need to be a database detective—discovering what's actually in there without prior knowledge.

### ResultSetMetaData

Perfect for building dynamic UIs or exporting data to CSV. Gives you:

- Number of columns
- Column names and types
- Precision and scale information

### DatabaseMetaData

Your database's autobiography. Query it to discover:

- Available tables and views
- Column definitions
- Primary and foreign keys
- What SQL features the DBMS supports

**Example queries:** `dbmd.getTables(...)`, `dbmd.getColumns(...)`, `dbmd.getPrimaryKeys(...)`

---

## 💼 Transactions in JDBC

<aside>
💡

**By default, JDBC auto-commits every statement.** That means each SQL command is immediately permanent—no take-backs!

</aside>

For multi-step operations that should succeed or fail together, take control:

```java
conn.setAutoCommit(false);
try {
    // Execute multiple related updates
    // ...
    conn.commit();  // All good? Lock it in!
} catch (SQLException e) {
    conn.rollback();  // Something broke? Undo everything!
}
```

**Important:** Always restore auto-commit mode when you're done if you changed it.

---

## 🔧 Other JDBC Superpowers

- **Stored Procedures:** Call database procedures using `CallableStatement` with the syntax `{call proc(?,?)}`
- **Large Objects:** Handle BLOBs and CLOBs with specialized methods that stream data efficiently—perfect for images, documents, and large text

---

## 📝 SQLJ & Embedded SQL

### SQLJ: Compile-Time SQL for Java

Less dynamic than JDBC, but catches errors at compile time rather than runtime. Uses special syntax: `#sql { ... }` and iterators.

### Embedded SQL: The Classic Approach

Used in C, C++, COBOL, Fortran—the languages that built the enterprise world.

**Key features:**

- Prefix statements with `EXEC SQL`
- Host variables use `:` prefix
- Declare variables in a special section

**The Cursor Pattern:**

```
EXEC SQL DECLARE c CURSOR FOR 
  SELECT id, name FROM student WHERE tot_cred > :credit_amount;
EXEC SQL OPEN c;
EXEC SQL FETCH c INTO :sid, :sname;
EXEC SQL CLOSE c;
```

Think of cursors as iterators for your query results—fetch one row at a time, process it, move to the next.

### Updating Through Cursors

Declare your cursor `FOR UPDATE`, then modify the current row safely:

```
WHERE CURRENT OF c
```

---

## ⚙️ Functions, Procedures & Table Functions

SQL isn't just about queries—it's a full programming environment!

### Functions: Reusable Logic

Return a value and can be used anywhere in SQL expressions. Think of them as parameterized views.

```sql
CREATE FUNCTION dept_count(dept_name VARCHAR(20)) RETURNS INTEGER
BEGIN
  DECLARE d_count INTEGER;
  SELECT COUNT(*) INTO d_count 
  FROM instructor 
  WHERE instructor.dept_name = dept_name;
  RETURN d_count;
END;
```

Now you can write: `SELECT dept_count('Physics')` anywhere!

### Procedures: Actions with Side Effects

Don't return values directly, but can return data through OUT parameters. Invoke with `CALL`.

```sql
CREATE PROCEDURE dept_count_proc(
  IN dept_name VARCHAR(20), 
  OUT d_count INTEGER)
BEGIN
  SELECT COUNT(*) INTO d_count 
  FROM instructor 
  WHERE dept_name = dept_count_proc.dept_name;
END;
```

### Table Functions: Query Results as Tables

Return entire relations that can be used in `FROM` clauses via `TABLE(func(...))`.

---

## 🔄 Procedural Language Constructs

SQL:1999 brought real programming constructs into SQL itself:

**Flow Control:**

- `WHILE` and `REPEAT` loops
- `FOR` loops over query results
- `IF-THEN-ELSE` and `CASE` statements
- Local variables and exception handlers

**Example: Summing department budgets**

```sql
DECLARE n INTEGER DEFAULT 0;
FOR r AS SELECT budget FROM department DO
  SET n = n + r.budget;
END FOR;
```

Perfect for complex business logic that's best kept close to the data!

<aside>
📌

**Note:** Vendor dialects vary significantly. Always check your DBMS documentation for specific syntax.

</aside>

---

## 🌍 External Language Routines

Sometimes SQL isn't enough—you need the full power of C, Java, or other languages for specialized tasks like image processing, geospatial operations, or machine learning.

**Declaration example:**

```sql
LANGUAGE C EXTERNAL NAME '/path/to/proc'
```

<aside>
⚠️

**Security Warning**

External code runs in the database's address space. A bug or malicious code can:

- Corrupt the entire database
- Leak sensitive data
- Crash the DBMS

**Safer alternatives:**

- Use sandboxed languages (like Java in a JVM)
- Run external code in separate processes with IPC (slower but much safer)
</aside>

---

## ⚡ Triggers: Automatic Database Reactions

Triggers are like database ninjas—silently watching for events and springing into action automatically.

**Anatomy of a Trigger:**

- **Event:** INSERT, UPDATE, or DELETE
- **Timing:** BEFORE or AFTER the event
- **Level:** Per ROW or per STATEMENT
- **Condition:** Optional WHEN clause
- **Action:** What to do

### Example 1: Clean Data Before Saving

```sql
CREATE TRIGGER setnull_trigger
BEFORE UPDATE OF grade ON takes
REFERENCING NEW ROW AS nrow
FOR EACH ROW
WHEN (nrow.grade = ' ')
BEGIN ATOMIC
  SET nrow.grade = NULL;
END;
```

This normalizes blank grades to proper NULL values automatically—no messy data!

### Example 2: Maintain Student Credits

```sql
CREATE TRIGGER credits_earned
AFTER UPDATE OF grade ON takes
REFERENCING NEW ROW AS nrow OLD ROW AS orow
FOR EACH ROW
WHEN nrow.grade <> 'F' AND nrow.grade IS NOT NULL
  AND (orow.grade = 'F' OR orow.grade IS NULL)
BEGIN ATOMIC
  UPDATE student 
  SET tot_cred = tot_cred + (
    SELECT credits FROM course WHERE course.course_id = nrow.course_id)
  WHERE [student.id](http://student.id) = [nrow.id](http://nrow.id);
END;
```

When a student passes a course, their total credits update automatically. No manual tracking needed!

### Statement-Level Triggers

Use `FOR EACH STATEMENT` with transition tables (`REFERENCING NEW TABLE/OLD TABLE`) for efficient bulk operations—much faster than processing millions of rows individually.

---

## 🚫 When NOT to Use Triggers

<aside>
🤔

**Stop! Consider alternatives first**

Triggers seem powerful, but they're often the wrong tool for the job.

</aside>

**Don't use triggers for:**

- **Summary maintenance** → Use materialized views with refresh policies instead
- **Replication** → Use your DBMS's built-in replication features
- **Complex workflows** → Use application code or stored procedures

**Why triggers can be problematic:**

- Unexpected side effects during bulk operations
- Cascade complexity (triggers firing other triggers)
- Debugging nightmares (invisible code execution)
- Performance issues with replication
- Makes database behavior unpredictable

**Better approach:** Use explicit methods and controlled stored procedures for predictable, maintainable behavior.

---

## ✅ Practical Recommendations & Best Practices

<aside>
🎯

**Your Database Programming Checklist**

✓ **Always use prepared statements** for any user input—no exceptions!

✓ **Wrap multi-step updates in transactions** with proper error handling

✓ **Use metadata APIs** to build dynamic, flexible applications

✓ **Keep heavy computation** in the right place: either controlled stored procedures or external services

✓ **Prefer built-in features** over custom trigger solutions

✓ **Document trigger side effects** clearly and add safeguards for bulk operations

✓ **Test error conditions** with rollback scenarios

</aside>

---

*You've completed Advanced SQL—armed with the tools to build robust, secure, and efficient database applications! 🚀*