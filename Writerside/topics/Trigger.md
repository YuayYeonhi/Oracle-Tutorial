# Trigger

## 1. BEFORE Trigger 
(1) _Description_

A BEFORE Trigger means that Oracle will fire this trigger before the INSERT / UPDATE /
DELETE operation is executed. 

(2) _Syntax_ 
```SQL
CREATE [ OR REPLACE ] TRIGGER trigger_name 
BEFORE INSERT / UPDATE [ OF column ] / DELETE ON table_name 
    [ FOR EACH ROW ] 
    [ WHEN ... ] 
DECLARE 
    -- variable declarations 
BEGIN 
    -- trigger code 
EXCEPTION 
    WHEN ... 
    -- exception handling 
END;
```
(3) _Parameters or Arguments_

1. `OR REPLACE`
Optional. If specified, it allows you to re-create the trigger is it already exists so that you can change the trigger definition without issuing a DROP TRIGGER statement.
2. `trigger_name`
The name of the trigger to create.

3. `BEFORE INSERT / UPDATE [ OF column ] / DELETE`
It indicates that the trigger will fire before the INSERT / UPDATE / DELETE operation is executed.

4. `table_name`
The name of the table that the trigger is created on.

(4) _Restrictions_

1. You can not create a BEFORE trigger on a view.
2. You can update the :NEW values in a BEFORE INSERT / UPDATE Trigger if `FOR EACH ROW` specified.
3. You can update the :OLD values in a BEFORE UPDATE / DELETE Trigger if `FOR EACH ROW` specified.

(5) `Example`
Example 1:
<code-block lang="sql">
CREATE OR REPLACE TRIGGER secure_employees
BEFORE INSERT ON employees
BEGIN
    IF (TO_CHAR(SYSDATE,'DY') IN ('星期六', '星期日'))  
        OR (TO_CHAR(SYSDATE,'HH24:MI') NOT BETWEEN '08:00' AND '18:00')
    THEN RAISE_APPLICATION_ERROR(-20500,'You may insert into EMPLOYEES table only during business hours.');
    END IF;
END;
/
</code-block>

![TriggerEg1.png](TriggerEg1.png)

Example 2:
<code-block lang="sql">
CREATE OR REPLACE TRIGGER secure_emp
BEFORE INSERT OR UPDATE OR DELETE ON emp
BEGIN
    IF (TO_CHAR(SYSDATE,'DY') IN ('SAT', 'SUN')) OR (TO_CHAR(SYSDATE, 'HH24') NOT BETWEEN '08' AND '18')
        THEN
            IF DELETING THEN
                RAISE_APPLICATION_ERROR(-20502,'You may delete from EMP table only during business hours.');
            ELSIF INSERTING THEN
                RAISE_APPLICATION_ERROR(-20500,'You may insert into EMP table only during business hours.');
            ELSIF UPDATING('SAL') THEN
                RAISE_APPLICATION_ERROR(-20503,'You may update SALARY only during business hours. ');
            ELSE
                RAISE_APPLICATION_ERROR(-20504,'You may update EMP table only during business hours.');
            END IF;
    END IF;
END;
/
</code-block>

Example 3:
<code-block lang="sql">
CREATE OR REPLACE TRIGGER restrict_salary
BEFORE INSERT OR UPDATE OF salary ON employees
FOR EACH ROW
BEGIN
    IF :NEW.title='Salesperson' AND :NEW.salary>500000
    THEN
        :NEW.salary:=500000;
    END IF;
END;
/ 
</code-block>

## 2. AFTER Trigger 

Example 1:

```SQL
CREATE OR REPLACE TRIGGER before_product_price_update 
AFTER UPDATE OF price ON products 
FOR EACH ROW  
WHEN (new.price < old.price * 0.75) 
BEGIN 
    dbms_output.put_line('product_id = ' || :old.product_id); 
    dbms_output.put_line('Old price = ' || :old.price); 
    dbms_output.put_line('New price = ' || :new.price); 
    dbms_output.put_line('The price reduction is more than 25%'); 
    INSERT INTO product_price_audit(product_id, old_price, new_price) 
    VALUES (:old.product_id, :old.price, :new.price); 
END before_product_price_update; 
/

```

![TriggerEg2.png](TriggerEg2.png)

## 3. Disable / Enable Trigger
(1) _Disable a Trigger_

   `ALTER TRIGGER trigger_name DISABLE;`

(2) _Disable all Triggers on a table_

   `ALTER TABLE table_name DISABLE ALL TRIGGERS;`

(3) _Enable a Trigger_

   `ALTER TRIGGER trigger_name ENABLE;`

(4) _Enable all Triggers on a table_

   `ALTER TABLE table_name ENABLE ALL TRIGGERS;`

## 4. DROP TRIGGER Statement
   `DROP TRIGGER trigger_name;`