# A Reminder in GROUP BY

Recall aggregate functions like `SUM()`, `COUNT()`, `MIN()`, etc. return a single value.
 Say you want the total amount of money made from sales. You'd do something like:

```sql
SELECT SUM(amount) FROM sales;
```

Now say you want the sum of sales by staff, with sums ordered highest to lowest:
```sql
SELECT staff_id, SUM(amount) FROM sales
GROUP BY staff_id
ORDER BY SUM(amount)
```

Note:
- Column(s) that you want to `GROUP BY` should be included in the `SELECT` statement
- If you want to order by what you're aggregating, you need to include the aggregate function in `ORDER BY`.
- The order of columns that you want to `GROUP BY` matters

```sql
-- Sales by staff, by customer
SELECT staff_id, customer_id, SUM(amount) FROM sales
GROUP BY staff_id, customer_id

-- Sales by customer, by sttaff
SELECT staff_id, customer_id, SUM(amount) FROM sales
GROUP BY staff_id, customer_id
```

[Source](https://www.udemy.com/course/the-complete-sql-bootcamp)
