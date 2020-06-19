# A Reminder in GROUP BY and HAVING

Recall aggregate functions like `SUM()`, `COUNT()`, `MIN()`, etc. return a single value.
 Say you want the total amount of money made from sales. You'd do something like:

```sql
SELECT SUM(amount) FROM sales;
```

Now say you want the sum of sales by staff, with sums ordered highest to lowest:
```sql
SELECT staff_id, SUM(amount) FROM sales
GROUP BY staff_id
ORDER BY SUM(amount) DESC;
```

Note:
- Column(s) that you want to `GROUP BY` should be included in the `SELECT` statement
- If you want to `ORDER BY` what you're aggregating, you need to `ORDER BY` the aggregate and not just the column.
- The order of columns that you want to `GROUP BY` matters.

```sql
-- Sales by staff, by customer
SELECT staff_id, customer_id, SUM(amount) FROM sales
GROUP BY staff_id, customer_id;

-- Sales by customer, by staff
SELECT staff_id, customer_id, SUM(amount) FROM sales
GROUP BY staff_id, customer_id;
```

### Filtering with Aggregates

Say you want the sales of the most recent hires (determined by those with a start date after 2020-01-01).
```sql
SELECT staff_id, SUM(amount) FROM sales
WHERE start_date > '2020-01-01'
GROUP BY staff_id;
```

Now you want to filter this list by the highest-performing staff (determined by those who have total sales exceeding $1,000).
```sql
SELECT staff_id, SUM(amount) FROM sales
WHERE start_date > '2020-01-01'
GROUP BY staff_id
HAVING SUM(amount) > 1000;
```
You can see in the above function that `WHERE` is used to filter on columns whereas `HAVING` is used to filter on aggregates.

[Source](https://www.udemy.com/course/the-complete-sql-bootcamp)
