#  Function 

## 1. Create Function
_Syntax_
```SQL
CREATE [OR REPLACE] FUNCTION function_name 
    [ (parameter [,parameter]) ] 
RETURN return_datatype 
IS | AS 
    [declaration_section] 
BEGIN 
    executable_section 
[EXCEPTION 
    exception_section] 
END [function_name]; 
```
When you create a procedure or function, you may define parameters. There are three types of parameters that can be declared:

1.  IN - The parameter can be referenced by the procedure or function. The value of the parameter can not be overwritten by the procedure or function.
2. OUT - The parameter can not be referenced by the procedure or function, but the value of the parameter can be overwritten by the procedure or function.
3. IN OUT - The parameter can be referenced by the procedure or function and the value of the parameter can be overwritten by the procedure or function. 

## 2.Example
(1)
```SQL
create or replace function circle_area(p_radius numbers)
return number
as
    v_pi number:=3.1416;
    v_area number;
begin
    v_area:=v_pi*power(p_radius,2);
    return v_area;
end circle_area;
/    
```
(2)
```SQL
create or replace function avaerafe_product_price(p_product_type_id in integer)
return number
as
    v_average_product_price number;
begin
    select avg(price)
    into v_average_product_price
    from products
    where product_type_id=p_product_type_id;
    return v_average_product_price;
end;
/
```
## 3. Function Call
### (1) Use variables
```SQL
variable sumSalary number
exec :g_area:=circle_area(5);
print g_area
```
### (2) Select ... From Dual
```SQL
select average_product_price(1) from dual;
select average_product_price(2) from dual;
```
### (3) Function Call in SQL 
```SQL
select product_type_id, average_product_price(product_type_id)
from products
group by product_type_id;
```
```SQL
select product_id, price
from products
where price>average_product_price(product_type_id);
```

_Legal Function-call Clause:_  
* Select Clause
* Where Clause
* Having Clause
* Order by Clause
* Group by Clause
* Values Clause (Insert)
* Set Clause (Update)

_Restriction of Function Call in SQL:_ 
* Functions Only. Procedures are not allowed.
* Data types of parameters and return value must be legal in SQL. Boolean, Record or Table is not allowed.
* No application in the default value or check constraint of creating table. 

### (4) Function Call in Function/Procedure 
```SQL
create or replace procedure show_average_price
as
    v_average_price number;
    cursor v_type_id_cursor is
        select distinct product_type_id
        from products
        order by product_type_id;
begin
    dbms_output.put_line('product typeid average price');
    for v_type_id in v_type_id_cursor
        loop
            v_average_price := average_product_price(v_type_id.product_type_id);
            dbms_output.put_line(v_type_id.product_type_id || ' ' || v_average_price);
        end loop;
end;
/
exec show_average_price
```

## 4. Drop Function 
_Syntax_
```SQL
DROP FUNCTION function_name;
```
_Example_
```SQL
DROP FUNCTION average_product_price;
```