# CLAUDE.md - O'Reilly Advanced SQL for Data Analysis

## Project Overview

This repository contains training materials for **O'Reilly's Advanced SQL for Data Analysis** course. It is an educational resource focused on teaching advanced SQL techniques for data analysis, analytics, and business intelligence.

**Primary Author**: Thomas Nield
**Repository**: https://github.com/thomasnield/oreilly_advanced_sql_for_data

## Repository Structure

```
.
├── thunderbird_manufacturing.db      # SQLite database with sample data
├── customer_order.sql                # SQL script for creating and populating database tables
├── advanced_sql_class_notes.md       # Complete course notes in Markdown
├── advanced_sql_class_notes.pdf      # Course notes in PDF format
├── advanced_sql_class_notes.docx     # Course notes in Word format
├── oreilly_advanced_sql_for_data_analysis.pptx  # Course presentation
├── outline.md                        # Course outline/agenda
└── CLAUDE.md                         # This file - AI assistant guide
```

## Database Schema

### Primary Database: `thunderbird_manufacturing.db`

The SQLite database contains sample data for a fictional manufacturing/retail business. Based on the SQL scripts and class notes, the main tables are:

#### CUSTOMER_ORDER
- `CUSTOMER_ORDER_ID` (INTEGER PRIMARY KEY)
- `CUSTOMER_ID` (INTEGER)
- `ORDER_DATE` (DATE)
- `PRODUCT_ID` (INTEGER)
- `QUANTITY` (INTEGER)

#### CUSTOMER (referenced in queries)
- `CUSTOMER_ID` (INTEGER PRIMARY KEY)
- `CUSTOMER_NAME` (VARCHAR)
- `ADDRESS` (VARCHAR)
- `STATE` (VARCHAR)

#### PRODUCT (referenced in queries)
- `PRODUCT_ID` (INTEGER PRIMARY KEY)
- `PRODUCT_NAME` (VARCHAR)
- `PRODUCT_GROUP` (VARCHAR)
- `PRICE` (DECIMAL/DOUBLE)

#### CALENDAR (referenced in queries)
- `CALENDAR_DATE` (DATE)

The database contains order data primarily from Q1 2017 (January-March 2017).

## Course Content & Structure

### Section I: Setup
- Installing SQLiteStudio
- Database setup and introduction

### Section II: Subqueries, Unions, and Advanced Aggregations (30 min)
**Topics covered:**
- Scalar subqueries for filtering and aggregation
- Multi-value subqueries with IN clauses
- Derived tables for efficient joins
- UNION and UNION ALL operations
- GROUP_CONCAT / STRING_AGG functions

**Key concepts:**
- When subqueries are more/less expensive than joins
- Best practices for avoiding unions (prefer CASE statements)
- Table aliasing for complex queries

### Section III: Regular Expressions (20 min)
**Topics covered:**
- Regex syntax and operators (literals, ranges, anchors, repeaters, wildcards)
- REGEXP operator in SQLite
- Pattern matching for data validation and filtering

**Key patterns:**
- Character ranges: `[A-Z]`, `[0-9]`, `[A-Z0-9]`
- Anchors: `^` (start), `$` (end)
- Repeaters: `{n}`, `{n,m}`, `+`, `*`, `?`
- Wildcards: `.` (any character)
- Grouping: `( )` and alternation: `|`

### Section IV: Advanced Joins (40 min)
**Topics covered:**
- INNER JOIN and LEFT JOIN review
- Creating temporary/volatile tables
- Regular expression joins
- Self joins for temporal comparisons
- CROSS JOIN for cartesian products
- Comparative joins with inequality operators

**Key use cases:**
- Regex joins for flexible discount/pricing rules
- Self joins for "previous day" or "rolling" calculations
- Cross joins to fill data gaps (every date × product combination)
- Comparative joins for cumulative aggregations

### Section V: Windowing Functions (40 min)
**Platform note**: Uses PostgreSQL (via Rextester online compiler) since SQLite lacks native window function support.

**Topics covered:**
- `PARTITION BY` for contextual aggregations
- `ORDER BY` within window functions
- Combining `PARTITION BY` and `ORDER BY`
- Window bounds: `ROWS BETWEEN` vs `RANGE BETWEEN`
- Rolling/moving windows

**Common patterns:**
- `MAX(col) OVER(PARTITION BY category)` - max per category
- `SUM(col) OVER(ORDER BY date)` - running total
- `AVG(col) OVER(PARTITION BY category ORDER BY date)` - rolling average per category

### Section VI: Programming with SQL (20 min)
**Languages covered:**
- Python (using SQLAlchemy)
- R (using DBI and RSQLite)
- Java/Scala/Kotlin (using JDBC and HikariCP)

**Key principles:**
- Always use parameterized queries to prevent SQL injection
- Connection pooling for production applications
- Proper resource management (closing connections)

## Development Conventions

### SQL Style
- Use uppercase for SQL keywords: `SELECT`, `FROM`, `WHERE`, `INNER JOIN`
- Table/column names in UPPERCASE: `CUSTOMER_ORDER`, `PRODUCT_ID`
- Use meaningful table aliases: `c1`, `c2` for self-joins, `cust_avgs` for derived tables
- Group by using ordinal positions: `GROUP BY 1, 2, 3`
- Prefer explicit column names over `SELECT *` in production queries

### Query Organization
- Use CTEs (Common Table Expressions) for complex multi-step queries
- Derived tables should be aliased with descriptive names
- Comment complex regex patterns inline
- Format long queries with proper indentation

### Date Handling
- Dates stored as ISO 8601 format: `'YYYY-MM-DD'`
- SQLite date functions: `date()`, `datetime()`, etc.
- Date arithmetic: `date(col, '+1 day')`, `date(col, '-1 day')`

### Temporary Tables
```sql
CREATE TEMP TABLE table_name AS
WITH cte AS (
    SELECT ...
)
SELECT * FROM cte
```

## AI Assistant Guidelines

### When Working with SQL Queries
1. **Test queries carefully**: Validate syntax for the specific database platform (SQLite vs PostgreSQL)
2. **Explain query logic**: Break down complex queries step-by-step
3. **Performance considerations**: Note when derived tables are more efficient than subqueries
4. **Platform differences**: Be aware of SQLite limitations (no native window functions, limited date functions)

### When Modifying Course Materials
1. **Maintain educational focus**: Keep examples clear and pedagogical
2. **Preserve existing formatting**: Match the style of class notes
3. **Test SQL examples**: Ensure all queries execute correctly against thunderbird_manufacturing.db
4. **Update all formats**: If modifying content, remember there are .md, .pdf, and .docx versions

### When Adding New Examples
1. **Use existing schema**: Build on CUSTOMER, PRODUCT, CUSTOMER_ORDER tables
2. **Date range**: Use dates within Q1 2017 for consistency
3. **Progressive complexity**: Start simple, build to advanced
4. **Include exercises**: Provide practice problems with solutions

### Common Tasks

#### Running SQL Queries
Since SQLite3 may not be installed, consider:
- Using Python with SQLAlchemy to execute queries
- Reading the SQL files directly to understand structure
- Recommending SQLiteStudio for interactive exploration

#### Validating Examples
When checking SQL examples from the notes:
```python
from sqlalchemy import create_engine, text

engine = create_engine('sqlite:///thunderbird_manufacturing.db')
conn = engine.connect()

stmt = text("YOUR_SQL_QUERY_HERE")
results = conn.execute(stmt)
for row in results:
    print(row)
```

#### Testing Regex Patterns
SQLite regex examples can be tested:
```sql
SELECT 'test_string' REGEXP 'pattern' -- Returns 1 (true) or 0 (false)
```

## Best Practices for SQL Education

### Query Complexity Progression
1. Start with single-table SELECT statements
2. Add WHERE clauses and basic filtering
3. Introduce simple JOIN operations
4. Progress to subqueries and derived tables
5. Advanced: windowing, regex joins, self joins

### Common Pitfalls to Avoid
- **Don't use SELECT * in joins**: Specify columns explicitly
- **Avoid subqueries in SELECT for every row**: Use derived tables when possible
- **Don't forget table aliases in self joins**: Required for disambiguation
- **Mind the WHERE vs ON in LEFT JOIN**: Affects null handling
- **Window function bounds**: Understand ROWS vs RANGE behavior

### Performance Tips
- Derived tables are often more efficient than correlated subqueries
- Cross joins can create massive result sets - always filter appropriately
- Indexes on foreign keys improve join performance
- UNION ALL is faster than UNION (no deduplication)

## File Formats & Tools

### Recommended Tools
- **SQLiteStudio**: GUI for SQLite database exploration
- **Rextester**: Online PostgreSQL compiler for window functions
- **Python + SQLAlchemy**: For programmatic database access
- **Text editors**: For viewing .sql and .md files

### Converting Between Formats
The course notes exist in multiple formats:
- **Source**: `advanced_sql_class_notes.md` (Markdown)
- **PDF**: For printing and offline reading
- **DOCX**: For editing in Microsoft Word

When updating content, maintain consistency across all three formats.

## Testing & Validation

### Before Committing Changes
1. Validate all SQL syntax against appropriate platform (SQLite or PostgreSQL)
2. Test queries return expected results
3. Check for SQL injection vulnerabilities in code examples
4. Verify regex patterns work as documented
5. Ensure educational clarity and correctness

### Database State
The `thunderbird_manufacturing.db` database is read-only for course purposes. Exercises may create temporary tables, but should not modify permanent tables.

## Common Queries Reference

### Basic Aggregation
```sql
SELECT PRODUCT_ID, SUM(QUANTITY) as TOTAL_QTY
FROM CUSTOMER_ORDER
GROUP BY PRODUCT_ID
```

### Derived Table Join
```sql
SELECT co.*, avg_qty
FROM CUSTOMER_ORDER co
INNER JOIN (
    SELECT CUSTOMER_ID, PRODUCT_ID, AVG(QUANTITY) as avg_qty
    FROM CUSTOMER_ORDER
    GROUP BY 1, 2
) avgs
ON co.CUSTOMER_ID = avgs.CUSTOMER_ID
AND co.PRODUCT_ID = avgs.PRODUCT_ID
```

### Self Join (Previous Day)
```sql
SELECT o1.*, o2.QUANTITY as PREV_DAY_QTY
FROM CUSTOMER_ORDER o1
LEFT JOIN CUSTOMER_ORDER o2
ON o1.CUSTOMER_ID = o2.CUSTOMER_ID
AND o1.PRODUCT_ID = o2.PRODUCT_ID
AND o2.ORDER_DATE = date(o1.ORDER_DATE, '-1 day')
```

### Cross Join (Fill Gaps)
```sql
SELECT CALENDAR_DATE, PRODUCT_ID, COALESCE(TOTAL_QTY, 0)
FROM (
    SELECT CALENDAR_DATE, PRODUCT_ID
    FROM CALENDAR CROSS JOIN PRODUCT
    WHERE CALENDAR_DATE BETWEEN '2017-01-01' AND '2017-03-31'
) all_combos
LEFT JOIN (
    SELECT ORDER_DATE, PRODUCT_ID, SUM(QUANTITY) as TOTAL_QTY
    FROM CUSTOMER_ORDER
    GROUP BY 1, 2
) totals
ON all_combos.CALENDAR_DATE = totals.ORDER_DATE
AND all_combos.PRODUCT_ID = totals.PRODUCT_ID
```

## Git Workflow

### Branch Strategy
- Main development on `master` branch
- AI assistant work on feature branches: `claude/*`
- Commit messages should be descriptive and concise

### Typical Workflow
```bash
# Check current status
git status

# Stage changes
git add <files>

# Commit with descriptive message
git commit -m "Update course notes with new regex examples"

# Push to remote
git push -u origin <branch-name>
```

### Commit Message Style
Based on repository history:
- Use imperative mood: "Update", "Add", "Fix", "Delete"
- Be specific but concise
- Examples: "Update ppt", "add social media handle", "fixes"

## Additional Resources

### Official Documentation
- SQLite: https://www.sqlite.org/docs.html
- PostgreSQL: https://www.postgresql.org/docs/
- SQLAlchemy: https://docs.sqlalchemy.org/
- R DBI: https://cran.r-project.org/web/packages/DBI/DBI.pdf
- JDBC: http://tutorials.jenkov.com/jdbc/index.html

### Related Libraries
- **Python**: SQLAlchemy, sqlite3
- **R**: DBI, RSQLite
- **Java**: JDBC, HikariCP, Hibernate, jOOQ, Speedment

## Security Considerations

### SQL Injection Prevention
Always use parameterized queries:

**Python (SQLAlchemy):**
```python
stmt = text("SELECT * FROM CUSTOMER WHERE CUSTOMER_ID = :id")
conn.execute(stmt, id=customer_id)
```

**Java (JDBC):**
```java
PreparedStatement stmt = conn.prepareStatement("SELECT * FROM CUSTOMER WHERE CUSTOMER_ID = ?");
stmt.setInt(1, customerId);
```

**R:**
```r
dbGetQuery(db, "SELECT * FROM CUSTOMER WHERE CUSTOMER_ID = ?", params = list(customer_id))
```

### Never Concatenate User Input
❌ **Bad:**
```python
query = f"SELECT * FROM CUSTOMER WHERE CUSTOMER_ID = {user_input}"
```

✅ **Good:**
```python
stmt = text("SELECT * FROM CUSTOMER WHERE CUSTOMER_ID = :id")
conn.execute(stmt, id=user_input)
```

## Quick Start for AI Assistants

1. **Understand the context**: This is an educational SQL training repository
2. **Check the database schema**: Review table structures in class notes
3. **Validate platform**: Note SQLite vs PostgreSQL differences
4. **Test before suggesting**: Ensure SQL examples work correctly
5. **Maintain educational quality**: Keep examples clear and well-commented
6. **Preserve formatting**: Match existing style in course materials

## Contact & Contributions

For issues or suggestions regarding course content, refer to the original repository:
https://github.com/thomasnield/oreilly_advanced_sql_for_data

---

**Last Updated**: 2025-11-13
**Database Version**: thunderbird_manufacturing.db (Q1 2017 data)
**Primary SQL Platforms**: SQLite 3.x, PostgreSQL (for windowing)
