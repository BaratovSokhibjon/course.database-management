# GitHub Copilot Instructions for Database Management and Practice Course

This repository is for studying and practicing concepts from the Database Management and Practice course at Inha University. To get the most out of GitHub Copilot, follow these guidelines:

## General Coding Practices
- **Write clean, readable SQL**: Use proper formatting and indentation.
- **Comment your queries**: Explain complex logic, joins, and business rules.
- **Use meaningful names**: For tables, columns, and stored procedures.
- **Document database schemas**: Include ER diagrams and table descriptions.
- **Version control DDL scripts**: Track schema changes and migrations.

## Copilot Usage Tips
- **Prompt Copilot with context**: Start comments with clear descriptions of your database task.
- **Review generated SQL**: Always verify queries for correctness and performance.
- **Ask for explanations**: Use Copilot to explain query execution plans and optimizations.
- **Use Copilot for repetitive tasks**: E.g., creating tables, inserting test data, writing procedures.

## Project Structure
This repository is organized by **database topics and concepts**:

### Core Database Topics
- `fundamentals/`: Basic database concepts, relational model, and ER modeling
- `sql/`: SQL queries from basic to advanced, stored procedures, triggers, and views
- `database-design/`: Normalization, schema design, and indexing strategies
- `advanced-topics/`: Transactions, concurrency control, and NoSQL concepts

### Supporting Materials
- `practical-projects/`: Real-world database projects and case studies
- `resources/`: Sample databases, lecture materials, and reference guides
- `assignments/`: Course assignments organized by topic

### File Organization
- Use `.sql` files for database scripts and queries
- Include sample data in `resources/sample-databases/`
- Document schemas with markdown files alongside SQL scripts
- Keep large datasets out of version control

## Database-Specific Guidelines

### SQL Development (`sql/`)
- Use consistent naming conventions (snake_case for tables/columns)
- Include query comments explaining business logic
- Test queries with sample data before submission
- Optimize queries for performance when working with large datasets

### Database Design (`database-design/`)
- Create ER diagrams using tools like draw.io or Lucidchart
- Document normalization steps and reasoning
- Include index design rationale
- Consider data integrity constraints

### Advanced Topics (`advanced-topics/`)
- Practice transaction scenarios with ACID properties
- Experiment with different isolation levels
- Compare SQL vs NoSQL approaches for different use cases

## Example Copilot Prompts
- "# Create a normalized database schema for an e-commerce system"
- "# Write a query to find the top 10 customers by total purchases"
- "# Create a stored procedure to update inventory levels"
- "# Design an index strategy for this query performance issue"
- "# Write a trigger to automatically update audit timestamps"
- "# Explain the execution plan for this complex join query"
- "# Create sample data for testing this database schema"

## Best Practices for Database Work
- **Always backup** before running DDL scripts
- **Use transactions** for multi-statement operations
- **Test with realistic data volumes** when possible
- **Document performance considerations** for complex queries
- **Include error handling** in stored procedures and functions

## Exclusions
- Do not commit large database files or backups
- Avoid hardcoding sensitive connection strings
- Keep production data out of the repository
- Ignore temporary files and query result exports

## Workflow Tips
- Create feature branches for major schema changes
- Use descriptive commit messages referencing database objects
- Include before/after performance metrics for optimizations
- Cross-reference related queries and procedures in documentation

---

*Edit this file to add more instructions as you discover useful Copilot workflows for database development.*
- "# Fill missing categorical values with mode"
- "# Verify all missing values handled"
- "# Create scatter plot"
- "# Calculate correlation coefficient"
- "# Train linear regression model"
- "# Build classification model"
- "# Perform K-means clustering"

## Topic-Specific Guidelines

### Data Analysis (`data-analysis/`)
- Focus on pandas, numpy, matplotlib, and seaborn
- Minimal data exploration - show dataset structure and key statistics only
- Use concise print statements to display results

### Machine Learning (`machine-learning/`)
- Use scikit-learn for traditional ML algorithms
- Show only essential metrics and results
- Separate model training, evaluation, and results into different cells

### Deep Learning (`deep-learning/`)
- Use TensorFlow or PyTorch
- Focus on code implementation over detailed explanations
- Show training progress with minimal output
- Display only final model performance

## Exclusions
- Do not commit large datasets (>10MB) - use `resources/datasets/` only for small reference files
- Ignore model checkpoints, outputs, and temporary files
- Avoid committing virtual environments and build artifacts
- Keep sensitive data and API keys out of the repository

## Workflow Tips
- Create topic-specific branches for major assignments
- Use descriptive commit messages that reference the topic area
- Keep notebooks focused on single concepts for better organization
- Cross-reference related notebooks in different topic areas when beneficial