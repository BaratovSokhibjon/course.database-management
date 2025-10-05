# Week 4 - RDB and Constraints

# 🏛️ The Relational Model: Foundation of Modern Databases

<aside>
🏆

In 1970, Dr. E.F. Codd published a paper that changed everything: "A Relational Model for Large Shared Data Banks". This revolutionary idea won him the ACM Turing Award and became the backbone of nearly every database you use today.

</aside>

---

# 📚 Speaking the Language: Informal vs Formal

When working with relational databases, we use everyday terms but they map to precise mathematical concepts:

| **What You Say** | **What It Really Is** |
| --- | --- |
| Table | **Relation** |
| Column Header | **Attribute** |
| All Possible Column Values | **Domain** |
| Row | **Tuple** |
| Table Definition | **Schema of a Relation** |
| Populated Table | **State of the Relation** |

---

# 🎯 Core Concepts Explained

## Schema: The Blueprint

**Notation:** `R(A₁, A₂, ..., Aₙ)`

- **R** = Relation name
- **A₁, A₂, ...** = Attributes (columns)

<aside>
🎓

**Example:** STUDENT(Name, Ssn, Class, Major)
This defines a student table with four attributes.

</aside>

## Tuple: A Single Record

A **tuple** is an ordered set of values: `t = <v₁, v₂, ..., vₙ>`

<aside>
👤

**Example tuple:** `<'John Smith', '123456789', 4, 'CS'>`
This represents one student: John Smith, SSN 123456789, year 4, CS major.

</aside>

## Domain: The Rules

A **domain** defines what values are valid for an attribute.

> **Example:** "USA_phone_numbers" = set of all valid 10-digit US phone numbers
> 

> 
> 

> **Another:** "Grade_points" = {0.0, 0.5, 1.0, 1.5, ..., 4.0}
> 

## Relation State: The Snapshot

**r(R)** represents the current population of a relation — all tuples that exist right now.

**Mathematical view:** `r(R) ⊆ dom(A₁) × dom(A₂) × ... × dom(Aₙ)`

### 🧮 Cartesian Product Example

For relation `R(A₁, A₂)` where:

- `dom(A₁) = {0, 1}`
- `dom(A₂) = {a, b, c}`

All possible combinations: `{<0,a>, <0,b>, <0,c>, <1,a>, <1,b>, <1,c>}`

A valid state might be: `{<0,a>, <0,b>, <1,c>}` — a subset of all possibilities.

---

# ⚡ Four Fundamental Characteristics

## 1. 🔀 Tuples Are Unordered

Rows have **no guaranteed sequence** in a relation. Whether you see tuple A before B or B before A doesn't matter — it's the same relation!

> Think of a bag of marbles, not a stack of cards.
> 

## 2. 📐 Attributes Are Ordered

Attributes **do** have a defined order in the schema. Values in each tuple correspond to this ordering.

> Alternative: self-describing tuples using attribute-value pairs (like JSON).
> 

## 3. ⚛️ Values Are Atomic

All values must be **indivisible** — no nested lists, no structures within structures. This is the "relational" in relational database!

✅ `Name = 'John Smith'`

❌ `Courses = ['CS101', 'MATH201', 'PHYS150']` — Not allowed!

## 4. 🎯 Tuple Notation

- **t[Aᵢ]** — Value of attribute Aᵢ in tuple t
- **t[Aᵢ, Aⱼ, ..., Aₖ]** — Subtuple with specific attributes only

---

# 🛡️ Constraints: The Rules That Keep Data Clean

Constraints are the guardrails that prevent bad data from entering your database.

## Three Categories

<aside>
🔧

**1. Inherent** — Built into the data model itself
Example: Relational model doesn't allow lists as values

</aside>

<aside>
📋

**2. Schema-based (Explicit)** — Defined in your schema
Example: Primary keys, foreign keys, NOT NULL

</aside>

<aside>
💼

**3. Application-based (Semantic)** — Business rules
Example: "No employee can work more than 56 hours/week across all projects"

</aside>

---

# 🔑 Keys: Uniquely Identifying Tuples

## Superkey (SK)

Any set of attributes that uniquely identifies tuples. No two tuples can have identical superkey values.

**Formal:** For any `t₁ ≠ t₂` in r(R): `t₁[SK] ≠ t₂[SK]`

## Key (K)

A **minimal superkey** — remove any attribute and it stops being unique.

> All keys are superkeys, but not all superkeys are keys!
> 

## Candidate Keys

All the possible keys for a relation.

<aside>
🚗

**Example:** CAR(State, Reg#, SerialNo, Make, Model, Year)

**Candidate Key 1:** {State, Reg#}
**Candidate Key 2:** {SerialNo}

Note: {SerialNo, Make} is a superkey but NOT a key — removing Make still leaves it unique!

</aside>

## Primary Key (PK)

The **chosen one** among candidate keys — underlined in schema notation.

**Selection guideline:** Pick the smallest candidate key (fewer attributes, simpler values).

**Critical role:** Provides tuple identity and is used to reference tuples from other relations.

---

# 🚫 Entity Integrity: No NULL Primary Keys

<aside>
⚠️

**Rule:** Primary keys CANNOT contain NULL values.

**Why?** The PK identifies individual tuples. If it's NULL, which tuple are we talking about?

</aside>

**Formal:** `t[PK] ≠ null` for every tuple t in r(R)

If your PK has multiple attributes, **none** of them can be NULL.

---

# 🔗 Referential Integrity: Maintaining Relationships

Foreign keys create connections between tables — the "relational" magic!

## The Players

**Referencing relation (R₁)** — Contains the foreign key

**Referenced relation (R₂)** — Contains the primary key being referenced

**Foreign Key (FK)** — Attribute(s) in R₁ pointing to PK in R₂

## The Rules

Every FK value must be:

1. An **existing PK value** in the referenced table, OR
2. **NULL** (only if FK isn't part of R₁'s own PK)

## Notation

Shown as directed arc: `R₁.FK → R₂.PK`

<aside>
🏢

**Example:** EMPLOYEE(Ssn, Name, **Dno**) references DEPARTMENT(**Dnumber**, Dname)

Every Dno in EMPLOYEE must exist in DEPARTMENT.Dnumber or be NULL.
Arrow: EMPLOYEE.Dno → DEPARTMENT.Dnumber

</aside>

---

# 🗃️ Relational Database Schema

**Complete definition:** `S = {R₁, R₂, ..., Rₙ, IC}`

- **S** = Database schema name
- **R₁, R₂, ...** = Individual relation schemas
- **IC** = Set of integrity constraints

**Database State:** `DB = {r₁, r₂, ..., rₙ}`

Each rᵢ is a state (population) of Rᵢ, and all states satisfy the constraints in IC.

---

# ✏️ Update Operations: Changing the Data

Three fundamental operations change your database:

**📥 INSERT** — Add a new tuple

**🗑️ DELETE** — Remove an existing tuple

**✏️ MODIFY/UPDATE** — Change attribute values in a tuple

These operations may **cascade** to maintain integrity or need to be grouped into **transactions**.

---

# 🚨 Constraint Violations: What Goes Wrong

## INSERT Violations

| **Constraint** | **Violation** | **Action** |
| --- | --- | --- |
| Domain | Value not in valid range | ❌ REJECT |
| Key | Duplicate key value | ❌ REJECT |
| Entity Integrity | NULL in primary key | ❌ REJECT |
| Referential Integrity | FK references non-existent PK | ❌ REJECT |

## DELETE Violations

Only **referential integrity** can be violated — when the deleted tuple's PK is referenced elsewhere.

**Your options:**

<aside>
🛑

**RESTRICT/REJECT** — Cancel the deletion

</aside>

<aside>
🌊

**CASCADE** — Delete all referencing tuples too
(Like dominos falling!)

</aside>

<aside>
⚪

**SET NULL** — Set referencing FKs to NULL
(Break the link but keep the tuples)

</aside>

## UPDATE Violations

### Updating a Primary Key

Equivalent to DELETE + INSERT — same options as DELETE apply.

### Updating a Foreign Key

Must ensure new FK value exists in referenced relation or is NULL.

### Updating Regular Attributes

Only domain and NOT NULL constraints apply — safest operation!

---

# 💼 Example: COMPANY Database Schema

A real-world example to tie it all together:

### Relations

**EMPLOYEE**(*Ssn*, Name, Address, Salary, Super_ssn, Dno)

**DEPARTMENT**(*Dnumber*, Dname, Mgr_ssn, Mgr_start_date)

**DEPT_LOCATIONS**(*Dnumber*, *Dlocation*)

**PROJECT**(*Pnumber*, Pname, Plocation, Dnum)

**WORKS_ON**(*Essn*, *Pno*, Hours)

**DEPENDENT**(*Essn*, *Dependent_name*, Sex, Bdate, Relationship)

### Key Relationships

- EMPLOYEE.Super_ssn → EMPLOYEE.Ssn *(employees supervise employees!)*
- EMPLOYEE.Dno → DEPARTMENT.Dnumber
- DEPARTMENT.Mgr_ssn → EMPLOYEE.Ssn
- WORKS_ON.Essn → EMPLOYEE.Ssn
- WORKS_ON.Pno → PROJECT.Pnumber
- PROJECT.Dnum → DEPARTMENT.Dnumber
- DEPENDENT.Essn → EMPLOYEE.Ssn

---

# 🎓 Key Takeaways

<aside>
✨

**Master these principles:**

🎯 Relations are **sets** (unordered tuples)
📐 Attributes provide **structure and meaning**
🛡️ Constraints maintain **data integrity**
✏️ Updates must preserve **validity**
🔗 Foreign keys establish **inter-table relationships**
⚪ NULL represents **missing/unknown** values (with restrictions)

</aside>

💡 **Pro tip:** When designing a database, think constraints first! They're easier to add during design than to enforce later when you already have messy data.

**🔧 SQL Implementation:**

- `CREATE TABLE` defines keys, NOT NULL, FK constraints
- `CREATE TRIGGER` and `CREATE ASSERTION` for semantic constraints
- `UNIQUE` keyword for candidate keys
- `FOREIGN KEY` clause with `ON DELETE`/`ON UPDATE` options