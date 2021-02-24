

Ｏracle sql



字符串拼接

select last_name||'s job_id is '||job_id as details from employees

| DETAILS                  |
| ------------------------ |
| Kings job_id is AD_PRES  |
| Kochhars job_id is AD_VP |

# 

## PL/SQL



```plsql
declare 
    v_sal employees.salary%type;
    v_email employees.email%type;
    v_hire_date employees.hire_date%type;
begin 
    select salary, email, hire_date into v_sal, v_email, v_hire_date from employees where employee_id = 100;
    dbms_output.put_line(v_sal||', '||v_email||', '||v_hire_date);
end;   
```

 



###  record type

**similar to class and object in java:**

```plsql
declare 
 　  type emp_record is record(
        v_sal employees.salary%type,
        v_email employees.email%type,
        v_hire_date employees.hire_date%type
　   );

    v_emp_record emp_record;
    
begin 
    select salary, email, hire_date into v_emp_record from employees where employee_id = 100;
    dbms_output.put_line(v_emp_record.v_sal||', '||v_emp_record.v_email||', '||v_emp_record.v_hire_date);
end;     
```

​    

```plsql
declare
    type salary_record is record(
        v_name varchar2(20),
        v_salary number(10,2)　--10位数，2位小数
    );
    v_sal_record salary_record;

begin
    v_sal_record.v_name := 'Ivana';
    v_sal_record.v_salary := 100000;
    dbms_output.put_line('name: '||v_sal_record.v_name||' salary: '||v_sal_record.v_salary);
end;   
```

​     



### %rowtype

wrap the whole role as a variable

```plsql
declare
    v_emp_record employees%rowtype;

begin
    select * into v_emp_record from employees where employee_id = 123;
    dbms_output.put_line('email: '||v_emp_record.email||' salary: '||v_emp_record.salary);
end;  
```



### update

```plsql
declare
    v_emp_id number(10);

begin
    v_emp_id := 123;
    update employees set salary = salary + 100 where employee_id = v_emp_id;
    dbms_output.put_line('successfully updated!');
end;
```

   

### if

query the salary of id 150, 

if >1000, print 'salary > 10000', 

if 5000-10000 -> '5000<=salary<10000'

else -> 'salary<5000'

```plsql
declare
    v_emp_id employees.employee_id%type;
    v_emp_salary employees.salary%type;

begin
    v_emp_id := 150;
    select salary into v_emp_salary from employees where employee_id = v_emp_id;
    if v_emp_salary >= 10000 then dbms_output.put_line('salary >= 10000');
    elsif v_emp_salary >= 5000 then dbms_output.put_line('5000 <= salary < 10000');
    else dbms_output.put_line('salary < 5000');
    end if;
end;   
```

  

-----------------------------

```plsql
declare
    v_emp_id employees.employee_id%type;
    v_emp_salary employees.salary%type;
    v_temp varchar2(50);

begin
    v_emp_id := 150;
    select salary into v_emp_salary from employees where employee_id = v_emp_id;
    if v_emp_salary >= 10000 then v_temp := 'salary >= 10000';
    elsif v_emp_salary >= 5000 then v_temp := '5000 <= salary < 10000';
    else v_temp := 'salary < 5000';
    end if;
    dbms_output.put_line(v_temp);
end;   
```

​     

### loop

```plsql
declare
    v_i number(5) := 1;

begin
    loop
        dbms_output.put_line(v_i);
    exit when v_i >= 100;
        v_i := v_i + 1; 
    end loop;       
end; 
```

 

```plsql

declare
    v_i number(5) := 1;

begin
    while v_i <= 100 loop
          dbms_output.put_line(v_i);     
          v_i := v_i + 1; 
    end loop;      
end;    
```



-----------------------------

```plsql
begin
    for c in 1..100 loop
        dbms_output.put_line(c);
    end loop;     
end;   
```



----------------------

```plsql
begin
    for c in reverse 1..100 loop
        dbms_output.put_line(c);
    end loop;     
end;
```

​      

--------------------

prime number in 2-100

```plsql
declare 
    v_i number(3) := 2;
    v_j number(3) := 2;
    v_flag number(1) := 1;

begin
    while v_i <= 100 loop
        while v_j <= sqrt(v_i) loop
            if v_i mod v_j = 0 then v_flag := 0;
            end if;
            v_j := v_j + 1;
        end loop;
        if v_flag = 1 then dbms_output.put_line(v_i);
        end if;
        v_j := 2;
        v_i := v_i + 1;
        v_flag := 1;
    end loop;    
end;
```

  

-----------------

```plsql
declare 
    v_i number(3) := 2;
    v_j number(3) := 2;
    v_flag number(1) := 1;

begin
    for v_i in 2..100 loop
        for v_j in 2..sqrt(v_i) loop
            if v_i mod v_j = 0 then v_flag := 0;
            end if;
        end loop;
        if v_flag = 1 then dbms_output.put_line(v_i);
        end if;
        v_flag := 1;
    end loop;    
end;      
```



-----------------

```plsql
declare 
    v_i number(3) := 2;
    v_j number(3) := 2;
    v_flag number(1) := 1;

begin
    for v_i in 2..100 loop
        for v_j in 2..sqrt(v_i) loop
            if v_i mod v_j = 0 then v_flag := 0;
            goto label;
            end if;
        end loop;
        <<label>>
        if v_flag = 1 then dbms_output.put_line(v_i);
        end if;
        
        v_flag := 1;
    end loop;    
end;           
```



print 1-100, when 50, exit loop and print 'finished'

```plsql
begin
    for v_i in 1..100 loop
        dbms_output.put_line(v_i);
        if v_i >= 50 then goto label;
        end if;
    end loop;    
    <<label>>
    dbms_output.put_line('finished');
end;
```



----------------------

```plsql
begin
    for v_i in 1..100 loop
        if v_i >= 50 then dbms_output.put_line('finished');
        exit;
        end if;
        dbms_output.put_line(v_i);
    end loop;    
end;         
```

​       

### cursor

to deal with multiple rows, similar to iterator in java

e.g. print salaries of all employees in No.80 department

```plsql
declare 
    v_sal employees.salary%type;
    v_id employees.employee_id%type;
    -- define
    cursor emp_sal_cursor is select employee_id, salary from employees where department_id = 80;

begin
    -- open
    open emp_sal_cursor;

	-- fetch
    fetch emp_sal_cursor into v_id, v_sal;

    while emp_sal_cursor%found loop --if value found, then loop
        dbms_output.put_line('id: '||v_id||' salary: '||v_sal); 
        fetch emp_sal_cursor into v_id, v_sal;
    end loop;

    --close
    close emp_sal_cursor;    
end;              
```

  

------------------------

**with record type**

```plsql
declare 
    type emp_record is record(
        v_sal employees.salary%type,
        v_id employees.employee_id%type
    );
    v_emp_record emp_record;
    
    -- define
    cursor emp_sal_cursor is select employee_id, salary from employees where department_id = 80;

begin
    -- open
 　 open emp_sal_cursor;
    -- fetch
    fetch emp_sal_cursor into v_emp_record;
    while emp_sal_cursor%found loop --if value found, then loop
        dbms_output.put_line('id: '||v_emp_record.v_id||' salary: '||v_emp_record.v_sal); 
        fetch emp_sal_cursor into v_emp_record;
    end loop;
    --close
    close emp_sal_cursor;    
end;                
```



-----------------------

with for loop: automatically handle cursor,no need to manually open, fetch, close...

```plsql
declare 
    type emp_record is record(
        v_sal employees.salary%type,
        v_id employees.employee_id%type
    );
    v_emp_record emp_record;

    -- define
    cursor emp_sal_cursor is select employee_id, salary from employees where department_id = 80;
begin
    for c in emp_sal_cursor loop
        dbms_output.put_line('id: '||c.employee_id||' salary: '||c.salary);  
    end loop;    
end;  
```

  

use cursor to increase salaries

salary　　　　　           percentage to increase

0-5000                               5%

5000-10000                     3%

10000-15000                   2%

15000 -                               1%



```plsql
declare 
    v_sal employees.salary%type;
    v_id employees.employee_id%type;
    v_temp number(4,2);
    
    -- define
    cursor emp_sal_cursor is select employee_id, salary from employees;

begin
    for c in emp_sal_cursor loop
        dbms_output.put_line('id: '||c.employee_id||' salary: '||c.salary);  
    end loop;    

    open emp_sal_cursor;
    fetch emp_sal_cursor into v_id, v_sal;
    while emp_sal_cursor%found loop
        if v_sal < 5000 then v_temp := 0.05;
        elsif v_sal < 10000 then v_temp := 0.03;
        elsif v_sal < 15000 then v_temp := 0.02;
        else v_temp := 0.01;
        end if;
        update employees set salary = salary * (1 + v_temp) where employee_id = v_id;
        fetch emp_sal_cursor into v_id, v_sal;
    end loop;
    close emp_sal_cursor;
end;          
```



with for loop

```plsql
declare 
    v_temp number(4,2);
    cursor emp_sal_cursor is select employee_id, salary from employees;
begin
    for c in emp_sal_cursor loop
       dbms_output.put_line('id: '||c.employee_id||' salary: '||c.salary);  
    end loop;    

	for c in emp_sal_cursor loop
        if c.salary < 5000 then v_temp := 0.05;
        elsif c.salary < 10000 then v_temp := 0.03;
        elsif c.salary < 15000 then v_temp := 0.02;
        else v_temp := 0.01;
        end if;

        update employees set salary = salary * (1 + v_temp) where employee_id = c.employee_id;
    end loop;    
end;   
```



```plsql
declare 
    v_temp number(4,2);
    cursor emp_sal_cursor is select employee_id, salary from employees;
begin
    for c in emp_sal_cursor loop
        dbms_output.put_line('id: '||c.employee_id||' salary: '||c.salary);  
    end loop;    

    for c in emp_sal_cursor loop
        if c.salary < 5000 then v_temp := 0.05;
        elsif c.salary < 10000 then v_temp := 0.03;
        elsif c.salary < 15000 then v_temp := 0.02;
        else v_temp := 0.01;
        end if;

        update employees set salary = salary * (1 + v_temp) where employee_id = c.employee_id;
    end loop;    
end;   
```



implicit cursor

increase the salary of a centain employee by 10, if the employee is not found, then print ＇employee not found＇

```plsql
begin
   update employees set salary = salary + 10 where employee_id = 1001;
   if sql%notfound then dbms_output.put_line('employee not found');
   end if;
end;   
```



### exception

**predefined exception**

```plsql
declare 
    v_salary employees.salary%type;
begin
    select salary into v_salary from employees where employee_id > 100;
    dbms_output.put_line(v_salary);
exception
    when too_many_rows then dbms_output.put_line('too many rows to print!!!');
    when others then dbms_output.put_line('other kinds of exceptions!!!');
end;
```



**non-predefined exception:** 

connect the oracle exception code with our self defined exception

exception:

```plsql
begin
    delete from employees where employee_id = 100;
end;

---------------------
ORA-02292: integrity constraint (WKSP_MOODLEMAO.DEPT_MGR_FK) violated - child record found
```

modified:

```plsql
declare 
    e_deleteid_exception exception;
    pragma exception_init(e_deleteid_exception, -2292);
begin
    delete from employees where employee_id = 100;
exception
    when e_deleteid_exception then dbms_output.put_line('integrity constraint (WKSP_MOODLEMAO.DEPT_MGR_FK) violated, cannot delete the record!!!');
    when others then dbms_output.put_line('other kinds of exceptions!!!');
end;

----------------------
integrity constraint (WKSP_MOODLEMAO.DEPT_MGR_FK) violated, cannot delete the record!!!

1 row(s) deleted.
```



**self defined exception**

```plsql
declare 
    e_sal_too_high_exception exception;
    v_salary employees.salary%type;
begin
    select salary into v_salary from employees where employee_id = 100;
    if v_salary > 10000 then raise e_sal_too_high_exception;
    end if;
exception
    when e_sal_too_high_exception then dbms_output.put_line('salary is too high!!!');
    when others then dbms_output.put_line('other kinds of exceptions!!!');
end;

--------------------------
salary is too high!!!

Statement processed.
```



--update salary of a certain employee 1001 : if salary < 300, +100; handle NO_DATA_FOUND, TOO_MANY_ROWS exceptions.

```plsql
declare 
    v_salary employees.salary%type;
    v_id employees.employee_id%type := 1001;
begin
    select salary into v_salary from employees where employee_id = v_id;
    if v_salary < 300 then update employees set salary = salary + 100 where employee_id = v_id;
    end if;
exception
    when no_data_found then dbms_output.put_line('cannot find the employee!!!');
    when too_many_rows then dbms_output.put_line('too many rows to print!!!');
    when others then dbms_output.put_line('other kinds of exceptions!!!');
end;
```



### function

has a rerturn value

```plsql
create or replace function hello_world 
return varchar2
is --declare
begin
    return 'hello world';
end;    
```

then the function hello_world will be stored

call the function

```plsql
select hello_world from dual;
```

or

```plsql
begin
    dbms_output.put_line(hello_world);
end;  
```



**function with param**

```plsql
create or replace function hello_world2(v_logo varchar2)
return varchar2
is --declare
begin
    return 'hello world2'||v_logo;
end;  
```

```plsql
select hello_world2('mao') from dual;  
```

```plsql
begin
    dbms_output.put_line(hello_world2('mao'));
end;  
```



create a function that returns the system date

```plsql
create or replace function getSysDate
return Date
is
    v_date Date;
begin
    v_date := sysdate;
    return v_date;
end;    
```

```plsql
select getSysDate from dual;
```



create a function that adds up two numbers

```plsql
create or replace function add_numbers(v_num1 number, v_num2 number)
return number
is
begin
    return v_num1 + v_num2;
end;
```

```plsql
select add_numbers(2,6) from dual;
```



create a function that has department id as param and returns the sum of salaries of the department 

```plsql
create or replace function sum_of_salaries(dept_id number)
return number
is
    v_sum number(10) := 0;
    cursor emp_sal_cursor is select salary from employees where department_id = dept_id;

begin
    for c in emp_sal_cursor loop
        v_sum := v_sum + c.salary;
    end loop;    
    return v_sum;
end;
```

```plsql
select sum_of_salaries(80) from dual; 
```



**out: can return multiple values**

create a function that has department id as param and returns **the sum of salaries of the department** and **the total number of the employees.**

```plsql
create or replace function sum_of_salaries_and_emp(dept_id number, count_emp out number)
return number
is
    v_sum number(10) := 0;
    cursor emp_sal_cursor is select salary from employees where department_id = dept_id;

begin
    count_emp := 0;
    for c in emp_sal_cursor loop
        v_sum := v_sum + c.salary;
        count_emp := count_emp + 1;
    end loop;    
    return v_sum;
end;
```

```plsql
declare
    v_num number(5) := 0;
begin
    dbms_output.put_line(sum_of_salaries_and_emp(80, v_num));
    dbms_output.put_line(v_num);
end;   
```



### procedure

doesn't have a return value

```plsql
create or replace procedure sum_of_salaries2(dept_id number, sum_sal out number)
is
    cursor emp_sal_cursor is select salary from employees where department_id = dept_id;
begin
    sum_sal := 0;
    for c in emp_sal_cursor loop
        sum_sal := sum_sal + c.salary;
    end loop;    
    dbms_output.put_line(sum_sal);
end;   
```

```plsql
declare
    v_num number(10) := 0;
begin
    sum_of_salaries2(80, v_num);
end; 
```



create a procedure:

increase the salary of employees in a certain department, if the hired date is earlier than 95, 5%, if 95-98, 3%, if later than 98, 1%

return the total increase (use out param).

```plsql
create or replace procedure sum_of_salaries2(dept_id number, sum_sal out number)
is
    cursor emp_sal_date_cursor is select employee_id, salary, hire_date from employees where department_id = dept_id;
    v_temp number(4,2);
begin
    sum_sal := 0;
    for c in emp_sal_date_cursor loop
        if to_char(c.hire_date, 'yyyy') < '1995' then v_temp := c.salary * 0.05;
        elsif to_char(c.hire_date, 'yyyy') < '1998' then v_temp := c.salary * 0.03;
        else v_temp := c.salary * 0.01;
        end if;
        
        update employees set salary = salary + v_temp where employee_id = c.employee_id;

        sum_sal := sum_sal + v_temp;
    end loop;    
    dbms_output.put_line(sum_sal);
end; 
```

```plsql
declare
    v_num number(10) := 0;
begin
    sum_of_salaries2(80, v_num);
end; 
```



### trigger

for each row

```plsql
create or replace trigger update_emp_trigger
after 
    update on employees
for each row
begin
    dbms_output.put_line('hello maomao');
end;        
```

```plsql
update employees set salary = salary + 100 where department_id = 80;      
```



only once

```plsql
create or replace trigger update_emp_trigger
after 
    update on employees
-- for each row
begin
    dbms_output.put_line('hello maomao');
end;
```



print old value and new value: :old.salary, :new.salary

```plsql
create or replace trigger update_emp_trigger
after 
    update on employees
for each row
begin
    dbms_output.put_line('old salary'||:old.salary||', '||'new salary'||:new.salary);
end;  
```



when delete from my_emp, do backup in my_emp_bak

```plsql
-- create a new table

create table my_emp
as
select employee_id, salary from employees;
```

```plsql
-- create an empty table for backup

create table my_emp_bak
as
select employee_id, salary from employees where 1 = 2 -- emply table 
```

```plsql
create or replace trigger delete_emp_trigger
before     
    delete on my_emp
for each row
begin
    insert into my_emp_bak values(:old.employee_id, :old.salary);
end;  
```

