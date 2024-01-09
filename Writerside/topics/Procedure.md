# Procedure

## 1. Create Procedure 
_Syntax_
```SQL
CREATE [OR REPLACE] PROCEDURE procedure_name 
    [ (parameter [,parameter]) ] 
IS 
    [declaration_section] 
BEGIN 
    executable_section 
[EXCEPTION 
    exception_section] 
END [procedure_name]; 
```
When you create a procedure or function, you may define parameters. There are three types of parameters that can be declared:

1. IN - The parameter can be referenced by the procedure or function. The value of the parameter can not be overwritten by the procedure or function.
2. OUT - The parameter can not be referenced by the procedure or function, but the value of the parameter can be overwritten by the procedure or function.
3. IN OUT - The parameter can be referenced by the procedure or function and the value of the parameter can be overwritten by the procedure or function.

## 2. Example 
### (1) Hello World
_Execute procedures_
```SQL
    create or replace procedure hello
as
    begin
    dbms_output.put_line('Hello World!');
end;
/
exec hello

Hello World!
```

<note>PL/SQL use exec <code>procedure's name</code>. in other environments, you can use Anonymous blocks like:

```SQL
begin
    hello
end;
/
```
</note>

### (2) IN - parameter
```SQL
create or replace procedure print_salary(v_employee_id in employees.employee_id%type)
as
    v_salary employees.salary%type;
begin
    select salary into v_salary 
    from employees 
    where employee_id = v_employee_id;
    dbms_output.put_line(v_salary);
end;
/
```

### (3) Out - parameter
```SQL
    create or replace procedure getSumSalary(v_salary out number)
as
begin
    select sum(salary) 
    into v_salary 
    from employees;
end;
/
variable g_salary number

exec getSumSalary(:g_salary)
```
out:

![Procedure-Out.png](Procedure-Out.png)
### (4) Multi-Parameter 
```SQL
create or replace procedure update_product_price(P_product_id IN products.product_id%type,p_factor IN number)
as
begin
    update products
    set price = price * p_factor
    where product_id = p_product_id;
    commit;
exception
    when others then
        rollback;
end update_product_price;
/
```
### (5)Dynamic SQL in Procedures 
```SQL
create or replace procedure def_rows(p_table_name IN VARCHAR2,p_rows_deleted OUT NUMBER)
is
begin
    execute immediate 'delete from ' || p_table_name;
    p_rows_deleted := sql%rowcount;
end;
/
variable deleted NUMBER
EXECUTE def_rows('products', :deleted)
print deleted
```

## 3. Debug Code
### (1) PLS-00103
```SQL
create or replace procedure hello
begin
    dbms_output.put_line('Hello World!');
end;
/
```
`Warning: Procedure created with compilation errors`

```SQL
show errors
Errors for PROCEDURE STORE.HELLO:
LINE/COL ERROR -------- ---------------------------------------------------------------------
2/1      PLS-00103: 出现符号 "BEGIN"在需要下列之一时：( ; is with authid as cluster compress order using compiled wrapped external deterministic parallel_enable pipelined 符号 "is" 被替换为 "BEGIN" 后继续。  
```

### (2) ORA-00904
```SQL
create or replace procedure update_customer_dob (p_customer_id integer,p_dob date) 
as 
begin 
    update customers 
    set dob=p_dobs 
    where customer_id=p_customer_id; 
end update_customer_dob;
/  
```
`Warning: Procedure created with compilation errors`

```SQL
show errors
Errors for PROCEDURE STORE.UPDATE_CUSTOMER_DOB: 
LINE/COL ERROR -------- --------------------------------------- 
6/11     PL/SQL: ORA-00904: "P_DOBS": 标识符无效 
5/3      PL/SQL: SQL Statement ignored 
```

## 4. Procedures Call 
### (1) In Procedures
```SQL
create or replace procedure printSumSalary
as
    v_sumSalary number;
begin
    getSumSalary(v_sumSalary);
    dbms_output.put_line(v_sumSalary);
end;
/
```
### (2) In Anonymous Blocks 
```SQL
declare
    v_sumSalary number;
begin
    getSumSalary(v_sumSalary);
    dbms_output.put_line(v_sumSalary);
end;
/
```

## 5. Drop Procedure 
Once you have created your procedure in Oracle, you might find that you need to remove it
from the database. 

_Syntax_
```SQL
DROP PROCEDURE procedure_name;
```
_Example_
```SQL
DROP PROCEDURE getSumSalary;
```
This example would drop the procedure called getSumSalary.