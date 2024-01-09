# Anonymous blocks

## 1. PL/SQL: Procedural Language/Structured Query Language
## 2. Code Structure 
 ```SQL
      [Declare] 
      --Variable & Cursor Definition 
      Begin
      --Main body, includes SQL and PL/SQL
      [Exception]
      --Exception Handling
      End;
      /
 ```
## 3. Anonymous blocks

   ### (1)Set serveroutput on before running an anonymous block.
 ```SQL
 set serveroutput on
 ```
   ### (2)dbms_output.put_line
   ```SQL
      begin
         dbms_output.put_line('Hello World');
      end;
      /
   ```
   ### (4) Variable Initialization During Definition
   ```SQL
      declare
         a varchar2(10):='ABC';
         b varchar2(10):='XYZ';
      begin
         dbms_output.put_line(a||b);
      end;
      /
   ABCXYZ
   ```
   ### (5) String Variables
   ```SQL
   declare
      lv_fixed char(40):='someting not quite long.';
      lv_variable varchar2(40):='someting not quite long.';
      lv_clob clob :='someting not quite long.';
   begin
      dbms_output.put_line('Fixed Length:'||length(lv_fixed));
      dbms_output.out_line('Varying Length:'||length(lv_variable));
      dbms_output.out_line('CLOB Length:'||length(lv_clob));
   end;
   /
   Fixed Length:40
   Varying Length:25
   CLOB Length:25
   ```
   ### (6) Date Variables
   ```SQL
   declare
    lv_date1 date:=sysdate;
    lv_date2 date:=lv_date1;
   begin
    dbms_output.put_line(to_char(lv_date1,'yyyy-mm-dd hh24:mi:ss'));
    dbms_output.put_line(to_char(trunc(lv_date2),'yyyy-mm-dd hh24:mi:ss'));
   end;
   /
   2023-03-07 16:38:21
   2023-03-07 00:00:00
   ```
   ### (7) Number Variables
   ```SQL
   declare
     lv_number1 number(6,1);
     lv_number2 number(15,2):=23333.33;
   begin
     lv_number1 := lv_number2;
     dbms_output.put_line(lv_number1);
   end;
   /
   23333.3
   ```
## 4. IF Statement

   ### (1) IF ... THEN ... END IF
   ```SQL
   declare
      a integer;
   begin
      a:=1;
      if a>5 then
        dbms_output.put_line(a+2);
      end if;
   end;
   /
   ```
   ### (2) IF ... THEN ... **ELSE** ... END IF
   ```SQL
   declare
      a integer;
   begin
      a:=1;
      if a>5 then
        dbms_output.put_line(a+2);
      else
        dbms_output.put_line(a-2);
      end if;
   end;
   /
   -1
   ```
   ### (3) IF ... THEN ... **ELSIF** ... THEN ... END IF
   ```SQL
   declare
      a integer;
   begin
      a:=1;
      if a>5 then
        dbms_output.put_line(a+2);
      elsif a<2 then
        dbms_output.put_line(a-2);
      end if;
   end;
   /
    -1
   ```
   ### (4) IF ... THEN ... ELSIF ... THEN ... ELSE ... END IF
   ```SQL
   declare
      a integer;
   begin
      a:=3;
      if a>5 then
        dbms_output.put_line(a+2);
      elsif a<2 then
        dbms_output.put_line(a-2);
      else
        dbms_output.put_line(a);
      end if;
   end;
   /
   3
   ```
   ### (5) Nested IF
   ```SQL
   declare
        v_count number:=2;
        v_area number:=100;
        v_message varchar2(50);
   begin
       if v_count > 0 then
           v_message := 'v_count is positive';
           if v_area > 0 then
               v_message := 'v_count and v_area are positive';
           end if;
        elsif v_count = 0 then
            v_message := 'v_count is zero';
        else
            v_message := 'v_count is negative';
        end if;
        dbms_output.put_line(v_message);
   end;
   /
   v_count and v_area are positive
   ```   
## 5. Loop

   ### (1) Loop ... End Loop (Exit when true)
   ```SQL
    declare 
        v_counter number:=0;
    begin
        loop
            dbms_output.put_line(v_counter);
            v_counter := v_counter + 1;
            exit when v_counter = 5;
        end loop;
    end;
    /
    0
    1
    2
    3
    4   
   ```
   ### (2) For ... Loop ... End Loop 
   ```SQL
    declare 
        v_counter number;
    begin
        for v_counter in 1..5 loop
            dbms_output.put_line(v_counter);
        end loop;
    end;
    /
    1
    2
    3
    4
    5   
   ```
   ### (3) While ... Loop ... End Loop
   ```SQL
    declare
        v_counter number:=0;
    begin
        while v_counter < 6 loop
            v_counter := v_counter + 1;
            dbms_output.put_line(v_counter);
        end loop;
    end;
    /
    1
    2
    3
    4
    5
    6   
   ```
## 6. SQL in PL/SQL

   ### (1) Select Statement
   ```SQL
    declare
        v_salary employees.salary%type;
    begin
        select salary into v_salary
        from employees
        where employee_id = 1;
        dbms_output.put_line(v_salary);
    end;
    /
    800000 
   ```
   ```SQL
    declare
        v_firstname employees.first_name%type;
        v_lastname  employees.last_name%type;
    begin
        select first name, last name
        into v_firstname,v_lastname
        from employees
        where employee_id = 1;
        dbms_output.put_line(v_firstname || ' ' || v_lastname);
    end;
    /
    James Smith
   ```
   ```SQL
    declare
        v_salary number;
    begin
        select sum(salary)
        into v_salary
        from employees;
        dbms_output.put_line(v_salary);
    end;
    /
    2050000
   ```
   <note>Note: Select statement in PL/SQL must return only one single row.</note> 
   
   ### (2) DML
   
   ```SQL
    declare
        v_product_id      products.product_id%type := 1;
        v_product_type_id products.product_type_id%type;
        v_average_price   products.price%type;
    begin
        select product_type_id
        into v_product_type_id
        from products
        where product_id = v_product_id;
        select avg(price)
        into v_average_price
        from products
        where product_type_id = v_product_type_id
          and product_id != v_product_id;
        update products
        set price=v_average_price
        where product_id = v_product_id;
    end;
    /   
   ```
## 7. Exception

   ### (1) Exception Catching 
   ```SQL    
   declare 
        v_width integer;
        v_length integer:=0;
        v_area integer:=6;
    begin
        v_width := v_area / v_length;
        dbms_output.put_line('Width: ' || v_width);
    exception
    when zero_divide then
        dbms_output.put_line('Error: Divide by zero');
    end;
    /
   Output:Divide by zero
   ```
   Without exception catching ...
   ```SQL
    declare
        v_width integer;
        v_length integer:=0;
        v_area integer:=6;
    begin
        v_width := v_area / v_length;
        dbms_output.put_line('Width: ' || v_width);
    end;
    /
   ORA-01476: divisor is equal to zero
   ORA-06512: at line 6
   ```
   ### (2) Exception: NO_DATA_FOUND
   Definition: A select statement returns no rows.
   ```SQL
    declare
        v_firstname employees.first_name%type;
        v_lastname  employees.last_name%type;
    begin
        select first_name, last_name
        into v_firstname,v_lastname
        from employees
        where employee_id = 6;
        dbms_output.put_line(v_firstname || ' ' || v_lastname);
    exception
        when no_data_found then
            dbms_output.put_line('cannot find the employee') ;
    end;
    /
    cannot find the employee
   ```
   ### (3) Exception: TOO_MANY_ROWS
   Definition: A select statement returns more than one row. 
   ```SQL
    declare
        v_firstname employees.first_name%type;
        v_lastname  employees.last_name%type;
    begin
        select first_name, last_name
        into v_firstname,v_lastname
        from employees
        where employee_id = 6;
        dbms_output.put_line(v_firstname || ' ' || v_lastname);
    exception
        when too_many_rows then
            dbms_output.put_line('Too many employees selected') ;
    end;
    /
    Too many employees selected
   ```
   ### (4) Exception: DUP_VAL_ON_INDEX
   Definition: Attempt to store a duplicate value or values in a database column that is constrained by a unique index.
   ```SQL
    begin
        insert into customers(customer_id, first_name, last_name, dob, phone) 
        VALUES (1,'John','Smith');
    exception
        when dup_val_on_index then
            dbms_output.put_line('Duplicate value on an index');
    end;
    /
    Duplicate value on an index   
   ```
   ### (5) Exception: Others
   Definition: All other errors.
   ```SQL
    declare
        v_firstname employees.first_name%type;
        v_lastname  employees.last_name%type;
    begin
        select first_name, last_name
        into v_firstname,v_lastname
        from employees
        where employee_id = 6;
        dbms_output.put_line(v_firstname || ' ' || v_lastname);
    exception
        when too_many_rows then
            dbms_output.put_line('Too many employees selected') ;
        when others then
            dbms_output.put_line('An exception occurred') ;
    end;
    /
    An exception occurred   
   ```