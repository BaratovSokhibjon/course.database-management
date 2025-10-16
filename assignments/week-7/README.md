# Week 7 Assignment: Education Company Database

## Overview
This assignment implements a complete SQLite database for an Education Company with 8 tables, including data population and visualization.

## Files Included

1. **education_database.ipynb** - Jupyter notebook containing:
   - Database connection and setup
   - Table creation (all 8 tables)
   - Data insertion (10 rows per table, 80 rows total)
   - Data visualization using Pandas DataFrames

2. **education_database_erd.mmd** - Mermaid.js ERD diagram source file

3. **ERD_Figure.png** - Entity-Relationship Diagram image

## Database Schema

The database consists of 8 tables:

### 1. Companies
- **Fields**: id, name, headquarters
- **Primary Key**: id
- **NOT NULL**: name

### 2. Branches
- **Fields**: id, company_id, location
- **Primary Key**: id
- **Foreign Key**: company_id → Companies(id)
- **NOT NULL**: location

### 3. Courses
- **Fields**: id, branch_id, name, duration
- **Primary Key**: id
- **Foreign Key**: branch_id → Branches(id)
- **NOT NULL**: name

### 4. Students
- **Fields**: id, course_id, name, age
- **Primary Key**: id
- **Foreign Key**: course_id → Courses(id)
- **NOT NULL**: name

### 5. Enrollment
- **Fields**: id, student_id, course_id, enrollment_date
- **Primary Key**: id
- **Foreign Keys**: student_id → Students(id), course_id → Courses(id)

### 6. Logging
- **Fields**: id, activity, timestamp
- **Primary Key**: id
- **NOT NULL**: activity, timestamp

### 7. Teacher
- **Fields**: id, branch_id, name, subject
- **Primary Key**: id
- **Foreign Key**: branch_id → Branches(id)
- **NOT NULL**: name

### 8. Department
- **Fields**: id, branch_id, name
- **Primary Key**: id
- **Foreign Key**: branch_id → Branches(id)
- **NOT NULL**: name

## How to Run the Notebook

1. Open the notebook in Google Colab or Jupyter:
   ```bash
   jupyter notebook education_database.ipynb
   ```

2. Run all cells sequentially to:
   - Connect to the database
   - Create all tables
   - Insert data
   - Visualize the results

## How to Generate ERD Image

### Option 1: Using Mermaid Live Editor
1. Go to https://mermaid.live
2. Copy the contents of `education_database_erd.mmd`
3. Paste into the editor
4. Click "Export" → "PNG" or "SVG"
5. Save as `ERD_Figure.png`

### Option 2: Using Mermaid CLI
```bash
npm install -g @mermaid-js/mermaid-cli
mmdc -i education_database_erd.mmd -o ERD_Figure.png
```

### Option 3: Using VS Code Extension
1. Install "Markdown Preview Mermaid Support" extension
2. Open `education_database_erd.mmd`
3. Preview and export as image

## Relationships

- Companies → Branches (One-to-Many)
- Branches → Courses (One-to-Many)
- Branches → Teacher (One-to-Many)
- Branches → Department (One-to-Many)
- Courses → Students (One-to-Many)
- Students ← → Courses via Enrollment (Many-to-Many)
- Logging (Independent table for system activities)

## Data Population

Each table contains exactly 10 meaningful rows with proper foreign key relationships:
- 10 Companies with different headquarters
- 10 Branches linked to companies
- 10 Courses offered at branches
- 10 Students enrolled in courses
- 10 Enrollment records
- 10 Logging entries
- 10 Teachers assigned to branches
- 10 Departments within branches

**Total: 80 rows across all tables**
