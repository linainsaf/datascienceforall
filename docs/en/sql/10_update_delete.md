# Update & Delete

In addition to inserting data into tables, you may also need to modify or delete existing data. The SQL language provides several commands for updating and deleting data within tables.

## Updating Data

To update data within a table, you can use the `UPDATE` command followed by the name of the table and the `SET` keyword. The `SET` keyword is followed by the column name you want to update, an equals sign, and the new value you want to set.

<br />

Here's the basic syntax for updating data in a table:

```sql
UPDATE table_name
SET column_name = new_value
WHERE condition;
```

In this syntax, the `WHERE` clause specifies which rows to update. Without a `WHERE` clause, all rows in the table would be updated.

<br />

Here's an example that updates the price of a product in a table called `products`:

```sql
UPDATE products
SET price = 19.99
WHERE product_id = 1234;
```

This statement updates the price column for the row where the product_id is equal to 1234.

## Deleting Data

To delete data from a table, you can use the `DELETE` command followed by the name of the table. If you want to delete only certain rows, you can use a `WHERE` clause to specify which rows to delete.

<br />

Here's the basic syntax for deleting data from a table:

```sql
DELETE FROM table_name
WHERE condition;
```

Here's an example that deletes a row from a table called `orders`:

```sql
DELETE FROM orders
WHERE order_id = 5678;
```

This statement deletes only the row where the `order_id` is equal to 5678.


## Summarize : creating, inserting, updating and deleting

Let's summarize the previous notions with an SQL example code to create a table, insert values into it, and update a field : 

```sql
-- Creating a table for products
CREATE TABLE products (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  category VARCHAR(50),
  price DECIMAL(8, 2)
);

-- Inserting data into the table
INSERT INTO products (id, name, category, price)
VALUES (1, 'Product A', 'Category 1', 10.99),
       (2, 'Product B', 'Category 2', 19.99),
       (3, 'Product C', 'Category 1', 5.99);

-- Updating the price of Product A
UPDATE products
SET price = 12.99
WHERE id = 1;

-- Deleting a record from the 'products' table
DELETE FROM products
WHERE product_id = 2;
```

In this example, we first create a table named `products` with four columns: `id`, `name`, `category`, and `price`. We define the `id` column as the primary key, meaning it uniquely identifies each row in the table.

<br />

Next, we insert three rows of data into the table using the `INSERT INTO` command. Each row represents a different product, with values for the `id`, `name`, `category`, and `price` columns.

<br />

Finally, we update the price of `Product A` using the `UPDATE` command. We specify the table we want to update (`products`), the field we want to update (`price`), and the new value we want to set `(12.99)`. We use the `WHERE` clause to specify which row(s) we want to update; in this case, we only want to update the row with an `id` of `1`, which corresponds to `Product A`.

<br />

Then, the `DELETE` command is used to remove the record from the `products` table where the value of the `product_id` field is equal to `2`. This will delete the second product from the table, which in this case is `Product B`.

## Deleting a single field in a row 

Here's an example of deleting a single field in a row of the products table:

```sql
-- Delete the description of the product with id 3
UPDATE products
SET description = NULL
WHERE id = 3;
```

In this example, we use the `UPDATE` command to modify the description field of the row with `id` equal to `3`. The `SET` keyword is used to specify the new value of the description field, which we set to `NULL` to delete the existing value.

<br />

The `WHERE` clause is used to specify which row(s) to update. In this case, we're only updating the row with `id` equal to `3`. By setting the description field to `NULL`, we effectively delete the value of that field for that particular row.


