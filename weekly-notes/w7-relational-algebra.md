# Week 7 - Relational Algebra & Calculus

## 🧮 Relational Algebra & Calculus: The Math Behind SQL

> 🎯 Ever wonder what happens under the hood when you write SQL? Relational algebra is the mathematical foundation that makes it all work. It's the secret language databases speak!

---

## ⚡ Relational Algebra: The Operations Toolkit

**Relational algebra** is a set of operations for manipulating relations. It's **closed** - operations on relations produce new relations!

Think of it as the assembly language of databases - everything SQL does can be expressed in these fundamental operations.

### 🔹 Unary Operations (Single Table)

#### σ (Sigma) - SELECT: Filtering Rows

Pick tuples that meet a condition:

**σ_DNO=4(EMPLOYEE)**

> "Give me employees in department 4"

> 💡 **Key property:** SELECT is commutative - you can chain conditions in any order!
> `σ_A(σ_B(R)) = σ_B(σ_A(R))`

#### π (Pi) - PROJECT: Choosing Columns

Select specific attributes and **remove duplicates**:

**π_FNAME,LNAME,SALARY(EMPLOYEE)**

> "Show me just names and salaries, no duplicates"

#### ρ (Rho) - RENAME: Changing Names

Rename relations or attributes for clarity:

**ρ_RESULT(FNAME,LNAME)(DEP5_EMPS)**

> "Call this relation RESULT with columns FNAME and LNAME"

### 🧩 Set Theory Operations

These work on **union-compatible** relations (same number of attributes, compatible types):

- **∪ (Union)** - Combine tuples from both relations, remove duplicates
- **∩ (Intersection)** - Only tuples in both relations
- **− (Difference)** - Tuples in first but not second
- **× (Cartesian Product)** - Combine every tuple from R with every tuple from S

> 🎨 **Example:** Find employees who work on project 1 OR project 2? Use UNION!
> 
> Find employees who work on BOTH projects? Use INTERSECTION!

> Warning: Cartesian Product produces a LOT of tuples! (If R has 10 rows and S has 20 rows, result has 200 rows)

### 🔗 Binary Operations (Two Tables)

#### ⨝ (Bowtie) - JOIN: The Power Connector

Combine related tuples from two relations:

**DEPARTMENT ⨝_Mgr_ssn=Ssn EMPLOYEE**

> "Match departments with their managers"

**Key insight:** JOIN produces *fewer* tuples than CARTESIAN PRODUCT because it only keeps matching pairs!

#### ÷ (Division) - The "For All" Operation

Find tuples in R that are associated with **all** tuples in S.

**Complex but powerful:** "Find employees who work on ALL projects in department 5"

### 🌟 Advanced Operations

#### Outer Joins: Keeping the Unmatched

Sometimes you want to keep tuples even when they don't match:

- **⟕ LEFT OUTER JOIN** - Keep all left tuples, pad missing right with NULL
- **⟖ RIGHT OUTER JOIN** - Keep all right tuples, pad missing left with NULL
- **⟗ FULL OUTER JOIN** - Keep everything from both sides

> 💼 **Use case:** Show all employees, even those without assigned departments
> (LEFT OUTER JOIN keeps employees with NULL department)

#### Aggregate Functions: Crunching Numbers

**F_COUNT_Ssn,AVERAGE_Salary(EMPLOYEE)**

> "Count employees and calculate average salary"

Operations: **SUM**, **AVG**, **COUNT**, **MIN**, **MAX**

### 🎯 The Complete Set

These 6 operations can express **any query:**

- ✅ SELECT (σ)
- ✅ PROJECT (π)
- ✅ UNION (∪)
- ✅ DIFFERENCE (−)
- ✅ RENAME (ρ)
- ✅ CARTESIAN PRODUCT (×)

Everything else (JOIN, INTERSECTION, etc.) can be built from these!

---

## 🧠 Query Examples: Algebra in Action

### Example 1: Find Research Department Employees

**Single expression:**
```
π_FNAME,LNAME,ADDRESS(σ_DNAME='Research'(DEPARTMENT) ⨝_DNUMBER=DNO EMPLOYEE)
```

**Procedural (step-by-step):**
```
RESEARCH_DEPT ← σ_DNAME='Research'(DEPARTMENT)
RESEARCH_EMPS ← RESEARCH_DEPT ⨝_DNUMBER=DNO EMPLOYEE
RESULT ← π_FNAME,LNAME,ADDRESS(RESEARCH_EMPS)
```

> Same result, two styles - nested expression vs. sequential steps!

### Example 2: Employees Without Dependents

**Single expression:**
```
π_LNAME,FNAME(π_SSN(EMPLOYEE) − π_ESSN(DEPENDENT))
```

**Translation:** "All employee SSNs MINUS SSNs that appear in DEPENDENT table = employees without dependents"

---

## 📐 Relational Calculus: What, Not How

While algebra tells you **HOW** to get data (step-by-step operations), calculus tells you **WHAT** you want.

**Declarative vs. Procedural** - SQL is actually based on relational calculus!

### Tuple Relational Calculus

Uses **tuple variables** to describe what you want:

```
{t.FNAME, t.LNAME | EMPLOYEE(t) ∧ t.SALARY > 50000}
```

> "Give me first and last names of tuples t from EMPLOYEE where salary exceeds $50K"

#### Quantifiers

- **∃ (Exists)** - "There exists at least one..."
- **∀ (For All)** - "For every..."

> 🎓 **Example:** Find employees with dependents:
> `{t | EMPLOYEE(t) ∧ ∃d (DEPENDENT(d) ∧ d.ESSN = t.SSN)}`
> 
> Translation: "Employees t such that there exists a dependent d with matching SSN"

### Domain Relational Calculus

Uses **domain variables** for individual attribute values:

```
{uv | ∃qrstuvwxyz (EMPLOYEE(qrstuvwxyz) ∧ q='John' ∧ r='B' ∧ s='Smith')}
```

> Variables represent individual fields, not whole tuples

---

## 🖼️ QBE: Query-By-Example

**QBE** is a visual query language based on domain calculus - fill in a table to build your query!

### How It Works

Instead of writing code, you fill in **example elements**:

**EMPLOYEE Table:**

| FNAME | LNAME | BDATE | ADDRESS |
| --- | --- | --- | --- |
| P._u | P._v | 'John' | 'Smith' |

> "**P.**" means "print this", "_u" and "_v" are example variables

### Key Features

- **Visual & Intuitive** - No complex syntax to memorize
- **Joins** - Use same variable in multiple tables
- **Aggregation** - CNT, AVG, MAX, MIN, SUM
- **Grouping** - Use .G. to group results
- **Condition Box** - For complex AND/OR logic

#### Example: Complex Query

Find employees working on project 1 OR project 2:

**WORKS_ON Table:**

| ESSN | PNO | HOURS |
| --- | --- | --- |
| P._x | _y |  |

**Condition Box:**
```
_y = 1 OR _y = 2
```

> 🛠️ **Where it lives:** IBM's QMF, Microsoft Access, Paradox
> 
> QBE was actually Microsoft Access's inspiration for its visual query designer!

---

## 💼 The COMPANY Database

All examples use this schema:

- **EMPLOYEE** - Employee information (Ssn, Name, Salary, Dno, Super_ssn)
- **DEPARTMENT** - Department details (Dnumber, Dname, Mgr_ssn)
- **DEPT_LOCATIONS** - Department locations
- **PROJECT** - Project information (Pnumber, Pname, Dnum)
- **WORKS_ON** - Who works on which projects (Essn, Pno, Hours)
- **DEPENDENT** - Employee dependents (Essn, Dependent_name, Relationship)

---

## 🎓 Key Takeaways

> ✨ **Master these concepts:**
> 
> 🧮 **Relational Algebra** - Procedural operations (SELECT, PROJECT, JOIN, etc.)
> 📐 **Relational Calculus** - Declarative specifications (tuple & domain)
> 🖼️ **QBE** - Visual query language
> ⚡ **Complete Set** - 6 operations can express any query
> 🔗 **Equivalence** - Algebra, calculus, QBE, and SQL all express the same queries

💡 **Pro tip:** When writing complex queries, think in relational algebra first - break the problem into small operations (filter, join, project). Then translate to SQL. This makes debugging much easier!

### 🎯 The Big Picture:

- **Relational Algebra** → What the database engine actually does
- **Relational Calculus** → What SQL is based on
- **SQL** → What you actually write
- **QBE** → Visual alternative to SQL

They're all mathematically equivalent - different ways to express the same thing!