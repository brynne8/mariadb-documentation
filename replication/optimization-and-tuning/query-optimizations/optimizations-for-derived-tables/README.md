# Optimizations for Derived Tables

Derived tables are subqueries in the `FROM` clause. Prior to [MariaDB 5.3](/kb/en/what-is-mariadb-53/)/MySQL 5.6, they were too slow to be usable. In [MariaDB 5.3](/kb/en/what-is-mariadb-53/)/MySQL 5.6, there are two
optimizations which provide adequate performance:

- [Condition Pushdown into Derived Table Optimization](/replication/optimization-and-tuning/query-optimizations/optimizations-for-derived-tables/condition-pushdown-into-derived-table-optimization/) — If a query uses a derived table (or a view), the first action that the que...
- [Derived Table Merge Optimization](/replication/optimization-and-tuning/query-optimizations/optimizations-for-derived-tables/derived-table-merge-optimization/) — MariaDB 5.3 introduced the derived table merge optimization
- [Derived Table with Key Optimization](/replication/optimization-and-tuning/query-optimizations/optimizations-for-derived-tables/derived-table-with-key-optimization/) — Since MariaDB 5.3, the optimizer can create an index and use it for joins with other tables
- [Lateral Derived Optimization](/replication/optimization-and-tuning/query-optimizations/optimizations-for-derived-tables/lateral-derived-optimization/) — Lateral Derived optimization, also referred to as "Split Grouping Optimization".