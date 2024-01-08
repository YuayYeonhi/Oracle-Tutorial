# Select Statement-2

## Function
### InitCap/Lower/Upper
```SQL
select first_name,last_name from customers;
```
![ILU1.jpg](ILU1.jpg)
```SQL
select lower(first_name),upper(last_name) from customers;
```
![ilu2.jpg](ilu2.jpg)
```SQL
select initcap('abc') from dual;
#OUT:Abc
```
###  Length/Substr/Instr/Replace/Concat(||)
```SQL
select last_name,length(last_name),substr(last_name,2,4),substr(last_name,3) 
from customers;
```
![lsirc.jpg](lsirc.jpg)
```SQL
select last_name,instr(last_name,'e'),instr(last_name,'e',4) from customers;
```
![lsirc2.jpg](lsirc2.jpg)
```SQL
select last_name,replace(last_name,'r','R')from customers;
```
![lsirc3.jpg](lsirc3.jpg)
```SQL
select first_name,last_name,concat(first_name,last_name)，first_name||' '||last_name 
from customers;
```
![lsirc4.jpg](lsirc4.jpg)
### ABS/SIGN
```SQL
select abs(-2),abs(10),abs(3) from dual;
##OUT:2 0 3

select sign(-2),sign(10),sign(3) from dual;
##OUT:-1 0 1
```
### CEIL/FLOOR/ROUND/TRUNC
```SQL
select ceil(2.6),ceil(2.3),ceil(2),cell(-2.6),cell(-2.3),cell(-2) from dual;
##OUT:3 3 2 -2 -2 -2

select floor(2.6),floor(2.3),floor(2),floor(-2.6),floor(-2.3),floor(-2) from dual;
##OUT:2 2 2 -3 -3 -2

select round(2.6),round(2.3),round(2),round(-2.6),round(-2.3),round(-2) from dual;
##OUT:3 2 2 -3 -2 -2

select trunc(2.6),trunc(2.3),trunc(2),trunc(-2.6),trunc(-2.3),trunc(-2) from dual;
##OUT:3 2 2 -3 -2 -2

select trunc(2.6),trunc(2.3),trunc(2),trunc(-2.6),trunc(-2.3),trunc(-2) from dual;
##OUT:2 2 2 -2 -2 -2
```

### POWER/EXP/LOG/LN/SQRT
```SQL
select power(2,16),exp(2),log(5,625),ln(2),sqrt(121) from dual;
###OUT:65536 7.38905609 4 2 11
```
### to_number/to_char/to_date
![char_number_date.jpg](char_number_date.jpg)

查询`dob`的日期在1971年的数据（用字符串比较）：
```SQL
select * from customers
where to_char(dob,'yyyy')='1971';
```
![date1.jpg](date1.jpg)

查询`dob`的日期在1971年的数据（用数字比较）：
```SQL
select * from customers
where to_number(to_char(dob,'yyyy'))=1971;
```
![date1.jpg](date1.jpg)

将日期转成数字：
```SQL
select to_number(to_char(dob,'yyyymm'))from customers;
```
![cnd.jpg](cnd.jpg)

`EXTRACT`函数从`dob`列中提取出生年份：
```SQL
select * from customers
where extract(year from dob)=1971;
```
![date1.jpg](date1.jpg)

输出当前月份：
```SQL
select to_char(sysdate,'mm')from dual;
##when:sysdate=2019-10-28 17:57:33  OUT:10
```

查找`dob`为1965-01-01的数据：
```SQL
select * from customers
where dob=to_date('19650101','yyyymmdd')
```
![date.jpg](date.jpg)

**_Note: default time is 0:0:0._**

查找`dob`为2月的数据：
```SQL
select * from customers
where to_char(dob,'mm')=2;
```
![date2.jpg](date2.jpg)

```SQL
select sysdate,sysdate-to_date('2020-10-2 0:0:0','yyyy-mm-dd hh24:mi:ss') Duartion
from dual;
```
### regexp_like

### count/sum/avg/max/min/median

## Group by Clause/Having Clause
###  Column must appear in the GROUP BY clause or be used in an aggregate function. 

### ORA-00937: not a single-group group function 
Cause: You tried to execute a SELECT statement that included a GROUP BY function (ie: MIN Function, MAX Function, SUM Function, COUNT Function), but was missing the GROUP BY clause.

### ORA-00934: group function is not allowed here
Cause: You tried to execute a SQL statement that included one of the group functions (ie: MIN Function, MAX Function, SUM Function, COUNT Function) in either the WHERE clause or the GROUP BY clause.

### Group by Clause and Having Clause 