# Desafio2-Power-BI
Resolvendo desafio proposto na DIO, utilizando Microsoft Azure, Power BI e MySQL Workbench.

## Plataforma
![Power BI](https://img.shields.io/badge/Power%20BI-000?style=for-the-badge&logo=power-bi)
![MySQL](https://img.shields.io/badge/MySQL-000?style=for-the-badge&logo=mysql)
![Azure](https://img.shields.io/badge/Azure-0089D6?style=for-the-badge&logo=microsoft-azure)


## Instruções de Entrega do Desafio

Descrição do desafio:

Fazer a integração e importação para o MySQL Workbench e para o Power BI do BD armazendo no Microsoft Azure. Processar e tratar os dados para realizar a analise.
Para a construção do projeto acesse o link https://github.com/julianazanelatto/power_bi_analyst , onde irá encontrar as informações necessárias para o desenvolvimento do projeto.


### Código do BD

```sql
create schema if not exists azure_company;

use azure_company;



select * from information_schema.table_constraints

where constraint_schema = 'azure_company';



CREATE TABLE employee(

Fname varchar(15) not null,

  Minit char,

  Lname varchar(15) not null,

  Ssn char(9) not null, 

  Bdate date,

  Address varchar(30),

  Sex char,

  Salary decimal(10,2),

  Super_ssn char(9),

  Dno int not null,

  constraint chk_salary_employee check (Salary> 2000.0),

  constraint pk_employee primary key (Ssn)

);



alter table employee modify Dno int not null default 1;



create table departament(

Dname varchar(15) not null,

  Dnumber int not null,

  Mgr_ssn char(9) not null,

  Mgr_start_date date, 

  Dept_create_date date,

  constraint chk_date_dept check (Dept_create_date < Mgr_start_date),

  constraint pk_dept primary key (Dnumber),

  constraint unique_name_dept unique(Dname),

  foreign key (Mgr_ssn) references employee(Ssn)

);



alter table departament add constraint fk_dept foreign key(Mgr_ssn) references employee(Ssn) on update cascade;



create table dept_locations(

Dnumber int not null,

Dlocation varchar(15) not null,

  constraint pk_dept_locations primary key (Dnumber, Dlocation)

);



alter table dept_locations 

add constraint fk_dept_locations foreign key (Dnumber) references departament(Dnumber)

on delete cascade

  on update cascade;



create table project(

Pname varchar(15) not null,

Pnumber int not null,

  Plocation varchar(15),

  Dnum int not null,

  primary key (Pnumber),

  constraint unique_project unique (Pname),

  constraint fk_project foreign key (Dnum) references departament(Dnumber)

);





create table works_on(

Essn char(9) not null,

  Pno int not null,

  Hours decimal(3,1) not null,

  primary key (Essn, Pno),

  constraint fk_employee_works_on foreign key (Essn) references employee(Ssn),

  constraint fk_project_works_on foreign key (Pno) references project(Pnumber)

);



create table dependent(

Essn char(9) not null,

  Dependent_name varchar(15) not null,

  Sex char,

  Bdate date,

  Relationship varchar(8),

  primary key (Essn, Dependent_name),

  constraint fk_dependent foreign key (Essn) references employee(Ssn)

);



insert into employee values ('John', 'B', 'Smith', 123456789, '1965-01-09', '731-Fondren-Houston-TX', 'M', 30000, 333445555, 5), ('Franklin', 'T', 'Wong', 333445555, '1955-12-08', '638-Voss-Houston-TX', 'M', 40000, 888665555, 5), ('Alicia', 'J', 'Zelaya', 999887777, '1968-01-19', '3321-Castle-Spring-TX', 'F', 25000, 987654321, 4), ('Jennifer', 'S', 'Wallace', 987654321, '1941-06-20', '291-Berry-Bellaire-TX', 'F', 43000, 888665555, 4), ('Ramesh', 'K', 'Narayan', 666884444, '1962-09-15', '975-Fire-Oak-Humble-TX', 'M', 38000, 333445555, 5), ('Joyce', 'A', 'English', 453453453, '1972-07-31', '5631-Rice-Houston-TX', 'F', 25000, 333445555, 5), ('Ahmad', 'V', 'Jabbar', 987987987, '1969-03-29', '980-Dallas-Houston-TX', 'M', 25000, 987654321, 4), ('James', 'E', 'Borg', 888665555, '1937-11-10', '450-Stone-Houston-TX', 'M', 55000, NULL, 1);



insert into dependent values (333445555, 'Alice', 'F', '1986-04-05', 'Daughter'), (333445555, 'Theodore', 'M', '1983-10-25', 'Son'), (333445555, 'Joy', 'F', '1958-05-03', 'Spouse'), (987654321, 'Abner', 'M', '1942-02-28', 'Spouse'), (123456789, 'Michael', 'M', '1988-01-04', 'Son'), (123456789, 'Alice', 'F', '1988-12-30', 'Daughter'), (123456789, 'Elizabeth', 'F', '1967-05-05', 'Spouse');



insert into departament values ('Research', 5, 333445555, '1988-05-22','1986-05-22'), ('Administration', 4, 987654321, '1995-01-01','1994-01-01'), ('Headquarters', 1, 888665555,'1981-06-19','1980-06-19');



insert into dept_locations values (1, 'Houston'), (4, 'Stafford'), (5, 'Bellaire'), (5, 'Sugarland'), (5, 'Houston');



insert into project values ('ProductX', 1, 'Bellaire', 5), ('ProductY', 2, 'Sugarland', 5), ('ProductZ', 3, 'Houston', 5), ('Computerization', 10, 'Stafford', 4), ('Reorganization', 20, 'Houston', 1), ('Newbenefits', 30, 'Stafford', 4);



insert into works_on values (123456789, 1, 32.5), (123456789, 2, 7.5), (666884444, 3, 40.0), (453453453, 1, 20.0), (453453453, 2, 20.0), (333445555, 2, 10.0), (333445555, 3, 10.0), (333445555, 10, 10.0), (333445555, 20, 10.0), (999887777, 30, 30.0), (999887777, 10, 10.0), (987987987, 10, 35.0), (987987987, 30, 5.0), (987654321, 30, 20.0), (987654321, 20, 15.0), (888665555, 20, 0.0);
```


### Importação do BD na Microsoft Azure para o MySQL Workbench

Depois de importar o BD, criei um model para ver as relações e como está distribuido os dados.


![MySQL Workbench](https://github.com/lucasfukuta/Desafio2-Power-BI/blob/main/MySQL%20trat.png)

### Importação do BD na Microsoft Azure para o Power BI

Nessa parte realizei o tratamento e a limpeza dos dados 


![PowerBI](https://github.com/lucasfukuta/Desafio2-Power-BI/blob/main/PowerBI%20trat.png)










