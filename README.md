# sql-demo
### This demo shows how to write SQL(Oracle) to find eployees with max salary for each department
#### STEP 1: create the following 2 tables: CW_EMP(id, name, salary, did), CW_DEPT(id, name)
create table CW_DEPT  
( id number(10) not null,  
name VARCHAR2(50) not null,  
constraint DEPT_PK PRIMARY KEY(id)  
);  
create table CW_EMP  
(id number(10) not null,  
name VARCHAR2(50) not null,  
salary number(10,2) not null,  
did number(10) not null,  
constraint emp_pk PRIMARY KEY(id),  
constraint fk_dept FOREIGN KEY(did) references CW_DEPT(id)  
);  
#### STEP 2: insert some sample data to tables
insert into CW_DEPT values(1, 'IT');  
insert into CW_DEPT values(2, 'HR');  
insert into CW_DEPT values(3, 'SALES');  
insert into CW_DEPT values(4, 'CUSTOMER SERVICE');  
  
insert into CW_EMP values(1, 'CHRIS1', 100, 1);  
insert into CW_EMP values(2, 'CHRIS2', 200, 1);  
insert into CW_EMP values(3, 'CHRIS3', 200, 1);  
insert into CW_EMP values(4, 'CHRIS4', 500, 2);  
insert into CW_EMP values(5, 'CHRIS5', 500, 2);  
insert into CW_EMP values(6, 'CHRIS6', 600, 2);  
insert into CW_EMP values(7, 'CHRIS7', 900, 3);  
insert into CW_EMP values(8, 'CHRIS8', 800, 3);  
#### STEP 3: list min/max/avg salary and count for each department
&nbsp;&nbsp;select d.name as dName, min(e.salary) as minSal, max(e.salary) as maxSal, avg(e.salary) as avgSal, count(*) as cnt   
&nbsp;&nbsp;&nbsp;&nbsp;from CW_EMP e left join CW_DEPT d on d.id=e.did  
&nbsp;&nbsp;&nbsp;&nbsp;group by d.name;  
  
**_Result:_**  
* IT    100   200   166.67  3  
* SALES 800   900   850     2  
* HR    500   600   533.33  3  
#### STEP 4: based on previous step, further list employee with the max salary  
**_Use sub query:_**  
&nbsp;&nbsp;select e.name as eName, e.salary, d.name as dName  
&nbsp;&nbsp;&nbsp;&nbsp;from CW_EMP e left join CW_DEPT d on d.id=e.did  
&nbsp;&nbsp;&nbsp;&nbsp;where (d.name, e.salary) in  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**(select  d.name as dept_name, max(e.salary) as max_salary  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;from CW_EMP e left join CW_DEPT d on d.id=e.did  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;group by d.name)**;  
        
**_Use inner join:_**  
&nbsp;&nbsp;select e.name as eName, e.salary, d.name as dName  
&nbsp;&nbsp;&nbsp;&nbsp;from CW_EMP e left join CW_DEPT d on d.id=e.did    
&nbsp;&nbsp;&nbsp;&nbsp;inner join   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**(select max(e.salary) as maxSal, d.name as dName    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;from CW_EMP e left join CW_DEPT d on d.id=e.did    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;group by d.name)** grouped on d.name=grouped.dName and e.salary=grouped.maxSal;  
        
**_Result:_**  
* CHRIS3 200 IT  
* CHRIS2 200 IT  
* CHRIS7 900 SALES  
* CHRIS6 600 HR  
