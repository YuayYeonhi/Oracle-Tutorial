# Select Statement-1

## Basic Select Statement
最普通的查询
```SQL
select customer_id,first_name,dob,phone 
from customers;
```
查询表中所有的数据
```SQL
select * from customers;
```
## distinct
查询`customer_id`不同的数据
```SQL
select distinct customer_id from purchases;
```

## like
找到`first_name`中含有o的数据
```SQL
select * from customers
where first_name like '_o%';
```
查询`name`中含有`\`的数据
```SQL
select promotion_id,name from promotions
where name like '%\%%' escape '\';
```

## is null
查询表中`dob`为空的数据
```SQL
select * from customers
where dob is null;
```

## nvl

```SQL
select customer_id,first_name,last_name,dob,
nvl(phone,'Unknown phone number')as phone_number
from customers;
```

## in 
```SQL
select * from customers
where customer_id in (2,3,5,6);
```
```SQL
select * from customers
where customer_id not in (2,3,5,6);
```

## between and
```SQL
select * from customers
where customer_id between 2 and 6;
```
## and\or\not
```SQL
select * from customers
where(customer_id = 2 or customer_id >= 4)
and extract(month from dob) > 2;
```

## order by
```SQL
select * from customers
order by last_name;

select * from customers
order by 1;

select * from customers
order by last_name desc;
```

## Inner join
```SQL
select * from customers c,purchases p
where c.customer_id = p.customer_id;

select c.*, p.product_id, p.quantity
from customers c,purchases p
where c.customer_id=p.customer_id;

select c.*, p.product_id, p.quantity 
from customers c inner join purchases p
on c.customer_id = p.customer_id;

select *
from customers c inner join purchases p
using(customer_id);
```

```SQL
select c.*,p.product_id,p.quantity
from customers c inner join purchases p
using(customer_id);
```
**_using子句列部分不能有限定词_**
```SQL
select customer_id,c.first_name,c.last_name,c.dob,c.phone,p.product_id,p.quantity
from customers c inner join purchases p
using(customer_id);
```
## Outer join
```SQL
select p.name,pt.name
from products p left outer join product_types pt
using(product_type_id);

select p.name,pt.name
from product p right outer join product_types pt
using(product_type_id);

select p.name,pt.name
from product p full outer join product_types pt
using(product_type_id);
```

## Pseudo Column
```SQL
select sysdate from dual;
##SYSDATE
##----------------------
##2019-09-28 17:57:33

select sysdate-1/24 from dual;
##SYSDATE
##----------------------
##2019-09-28 16:57:33

select sysdate,sysdate - interval '1' day from dual;
##SYSDATE                 SYSDATE - INTERVAL '1' DAY
-----------------------  ---------------------------
##2019-09-28 17:57:33     2019-09-27 17:57:33

select stsdate,sysdate - interval '7' hour from dual;
##SYSDATE                 SYSDATE-INTERVAL'7'HOUR
##---------------------  ---------------------------
##2019-09-28 17:57:33     2019-09-28 10:57:33 
```
second\minute\hour\day\month\year