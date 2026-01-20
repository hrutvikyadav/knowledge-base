---
id: SQL
aliases:
  - SQL
tags: []
---

# SQL

## FETCH Data from database tables

- Using select statement
- fetch by row or colums or tables
- can fetch from multiple tables using `joins` - discussed later

syntax
```
SELECT {* / [DISTINCT] column / expression [alias] } FROM {table};
```

```sql
-- SELECT InvoiceId, TrackId, UnitPrice/2 as PriceInRup FROM invoice_items;
-- OR for an alias with spaces i.e. multiple words, wrap in single quotes 👇
-- SELECT InvoiceId, TrackId, UnitPrice/2 'Price In Rupees' FROM invoice_items;
SELECT * FROM invoice_items;

```
## Types of functions

### Single row functions
- works with one row
- return one result per row

#### Number functions
For arithmetic operations on data
1. ROUND
```sql
SELECT ROUND(69.421,2);
--           ^      ^
--        operand  digits-to-round-upto
-- rounds up or down to closest value
-- res -> 69.420

SELECT ROUND(69.419,2);
-- res -> 69.420
```
2. TRUNC
```sql
SELECT TRUNCATE(69.420, 0);
-- res -> 69
```
3. MOD

#### Conditional functions
1. CASE
```sql
SELECT name, job, salary,
CASE job WHEN 'MANGER' THEN 1.20*salary
         WHEN 'ANALYST' THEN 1.15*salary
         WHEN 'CLERK' THEN 1.10*salary
         ELSE salary
         END 'Revised Salary'
FROM emp_tab;
```

#### Character functions
1. UPPER
2. LOWER
3. INITCAP
4. CONCAT
5. SUBSTR
6. LENGTH
7. INSTR
8. LPAD/RPAD
9. TRIM
10. REPLACE

### Multiple row functions
- work with group of rows
- return one result per group

