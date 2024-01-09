# Cursor 

## 1. Declare a Cursor 
A cursor is a `SELECT` statement that is defined within the declaration section of your PLSQL code.

### (1) Cursor without parameters 
Declaring a cursor without any parameters is the simplest cursor.

_Syntax_
```SQL
CURSOR cursor_name
IS
    SELECT _statement
```
_Example_
```SQL
cursor c_customers is
    select customer_id from customers
    where dob is not null;
```
The result set of this cursor is all customer IDs whose dob is not a null value.
### (2) Cursor with parameters
As we get more complicated, we can declare cursors with parameters.

_Syntax_
```SQL
CURSOR cursor_name(parameter_list)
IS
    SELECT _statement
```
_Example_
```SQL
cursor c_products(p_product_type_id IN products.product_type_id%type)
is
    select name,price from products
    where product_type_id=p_product_type_id
    order by price desc;
```
The result set of this cursor is all names and prices whose product type ID matches the
p_product_type_id passed to the cursor via the parameter.

## 2. CURSOR FOR Loop
You would use a `CURSOR FOR LOOP` when you want to fetch and process every record in a
cursor. The CURSOR FOR LOOP will terminate when all of the records in the cursor have been
fetched.

_Syntax_
```SQL
FOR record_index in cursor_name
LOOP
    {...statements...}
END LOOP; 
```
_Example 1_ 
```SQL
declare
    cursor c_customers is
        select customer_id
        from customers
        where dob is not null
        order by dob;
begin
    for r_customers in c_customers
        loop
            dbms_output.put_line(r_customers.customer_id);
        end loop;
end;
/
```
Result:
```SQL
1
2
5
3
```
_Example 2_
```SQL
declare
    cursor c_products(p_product_type_id IN products.product_type_id%type)
        is
        select name, price
        from products
        where product_type_id = p_product_type_id
        order by price desc;
begin
    dbms_output.put_line('Type ID: 1');
    for r_products in c_products(1)
        loop
            dbms_output.put_line(r_products.name || ' ' || r_products.price);
        end loop;
    dbms_output.put_line('Type ID: 2');
    for r_products in c_products(2)
        loop
            dbms_output.put_line(r_products.name || ' ' || r_products.price);
        end loop;
end;
/
```
Result:
```Shell
Type ID: 1 
Chemistry 30 
Modern Science 19.95 
Type ID: 2 
Z Files 49.99 
Supernova 25.99 
2412: The Return 14.95 
Tank War 13.95
```

## 3. SELECT FOR UPDATE Statement 
The `SELECT FOR UPDATE` statement allows you to lock the records in the cursor result set.
You are not required to make changes to the records in order to use this statement. The
record locks are released when the next commit or rollback statement is issued. 

_Syntax_
```SQL
CURSOR cursor_name 
IS 
    select_statement 
    FOR UPDATE [OF column_list] [NOWAIT];
```
_Parameters or Arguments_
`column_list`
The columns in the cursor result set that you wish to update.
`NOWAIT`
Optional. The cursor does not wait for resources.

_Example_
```SQL
cursor c_product is 
    select price 
    from products 
    where product_type_id=1 
    for update; 
```

## 4. WHERE CURRENT OF Statement
If you plan on updating or deleting records that have been referenced by a `SELECT FOR
UPDATE` statement, you can use the WHERE CURRENT OF statement.

_Syntax_

The syntax for the `WHERE CURRENT OF` statement in Oracle/PLSQL is either:
```SQL
UPDATE table_name 
    SET set_clause 
    WHERE CURRENT OF cursor_name; 
```
OR
```SQL
DELETE FROM table_name 
WHERE CURRENT OF cursor_name; 
```
<note>The WHERE CURRENT OF statement allows you to update or delete the record that was last 
fetched by the cursor. </note>

_Example_
```SQL
declare 
    cursor c_products is 
        select price 
        from products 
        where product_type_id=1 
        for update; 
begin
    for r_products in c_products loop 
        update products 
        set price=r_products.price*2 
        where current of c_products; 
    end loop; 
end; 
/ 
```

## 5. Update/Delete by RowID
If you plan on updating or deleting records that have been referenced by a SELECT statement,
you can use the RowID pseudo-column.

_Syntax_
```SQL
declare 
    cursor c_products is 
        select rowid,price 
        from products 
        where product_type_id=1; 
begin 
    for r_products in c_products loop 
        update products 
        set price=r_products.price*2 
        where rowid=r_products.rowid; 
    end loop; 
end; 
/
```
