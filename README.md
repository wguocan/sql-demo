# sql-demo
### This demo shows how to write SQL to find eployee with max salary for each department
#### STEP 1: create the following 2 tables: EMPLOYEE(id, name, salary, department_id), DEPARTMENT(id, name)
create table DEPARTMENT  
( id number(10) not null,  
name VARCHAR2(50),  
constraint DEPT_PK PRIMARY KEY(name)  
);  
  
create table EMPLOYEE  
(id number(10) not null,  
name VARCHAR2(50) not null,  
salary number(10,2) not null,  
department_id number(10),  
constraint emp_pk PRIMARY KEY(id),  
constraint fk_dept FOREIGN KEY(department_id) references DEPARTMENT(ID)  
);  
#### STEP 2: insert some sample data to tables
insert into DEPARTMENT values(1, 'IT');  
insert into DEPARTMENT values(2, 'HR');  
insert into DEPARTMENT values(3, 'SALES');  
insert into DEPARTMENT values(4, 'CUSTOMER SERVICE');  
  
insert into EMPLOYEE values(1, 'CHRIS1', 100, 1);  
insert into EMPLOYEE values(2, 'CHRIS2', 200, 1);  
insert into EMPLOYEE values(3, 'CHRIS3', 200, 1);  
insert into EMPLOYEE values(4, 'CHRIS4', 500, 2);  
insert into EMPLOYEE values(5, 'CHRIS5', 500, 2);  
insert into EMPLOYEE values(6, 'CHRIS6', 600, 2);  
insert into EMPLOYEE values(7, 'CHRIS7', 900, 3);  
insert into EMPLOYEE values(8, 'CHRIS8', 800, 3);  
#### STEP 3: find max salary for each department and list department name and its max salary
  select max(e.salary) as max_salary, d.name as dept_name  
    from EMPLOYEE e left join DEPARTMENT d on d.id=e.department_id  
    group by d.name;  
    
#### STEP 4: based on previous step, further list employee name who has the max salary
select e.name, e.salary, d.name  
    from EMPLOYEE e left join DEPARTMENT d on d.id=e.department_id  
    inner join (  
      select max(e.salary) as max_salary, d.name as dept_name  
        from EMPLOYEE e left join DEPARTMENT d on d.id=e.department_id  
        group by d.name) g on d.name=g.dept_name and e.salary=g.max_salary;  
