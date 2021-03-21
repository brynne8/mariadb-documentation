# ColumnStore Create View

Creates a stored query in the MariaDB ColumnStore

## Syntax

```sql
CREATE
    [OR REPLACE]
    VIEW view_name [(column_list)]
    AS select_statement
```

Notes to CREATE VIEW:

- If you describe a view in MariaDB ColumnStore, the column types reported may not match the actual column types in the underlying tables. This is normal and can be ignored.
The following statement creates a customer view of orders with status:

```sql
CREATE VIEW v_cust_orders (cust_name, order_number, order_status) as
select c.cust_name, o.ordernum, o.status from customer c, orders o
where c.custnum = o.custnum;

```