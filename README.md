# MYSQL-JOURNEY

/*To determine that my tables were
uploaded successfully on MYSQL Workbench*/

SELECT * FROM 
netflix_titles;

SELECT * FROM 
social_media;

SELECT COUNT(time_spent)
FROM social_media;

SELECT COUNT(time_spent)
FROM social_media
WHERE age > 20;


--SQL SELECT

SELECT * FROM netflix_titles
LIMIT 3;

SELECT TOP 3 * FROM netflix_titles
WHERE Country='Mexico';

SELECT MIN(release_year)
FROM netflix_titles;


SELECT MAX(release_year)
FROM netflix_titles;


--SQL GROUP BY

SELECT MIN(release_year) AS Old_School, show_id
FROM netflix_titles
GROUP BY show_id;


SELECT COUNT(DISTINCT time_spent)
FROM social_media;

SELECT SUM(time_spent)
FROM social_media;
WHERE profession = 'software';


SELECT DISTINCT time_spent AS Distincttime_spent, platform,
FROM social_media;
SELECT AVG(release_year)
FROM netflix_titles
WHERE rating = 'TV-MA';


SELECT * FROM netflix_titles
WHERE release_year <> 2021;



SELECT * FROM 
netflix_titles
where country = 'Germany';


DELETE FROM netflix_titles
WHERE country='Germany';




SELECT time_spent,interests
FROM social_media
WHERE interests IS NULL;


SELECT director,title,cast
FROM netflix_titles
WHERE director IS NOT NULL;



UPDATE netflix_titles
SET director = 'Alfred Schmidt', title= 'Frankfurt'
WHERE cast = 'Midnight Mass';





SELET gender,platform,
FROM social_media
WHERE time_spent = 2;


--SQL ORDER BY

SELET gender,platform AS avenue
FROM social_media;
ORDER BY time_spent ASC;


WHERE time_spent = 2
ORDER BY platform DESC;


SELECT DISTINCT release_year AS Year
FROM netflix_titles;

SELECT *
FROM social_media
WHERE location = 'Australia' AND platform LIKE 'G%';

SELECT * FROM  netflix_titles
WHERE NOT Country = 'India';


SELECT * FROM netflix_titles
WHERE type = 'Move' AND country LIKE 'G%' OR director LIKE 'R%';


SELECT * FROM netflix_titles
WHERE release_year NOT BETWEEN 2015 AND 2020;


SELECT * FROM netflix_titles
WHERE listed_in NOT IN ('International TV Shows, Romantic TV Shows, TV Comedies', 'Children & Family Movies');



SELECT COUNT(age), platform
FROM  social_media
GROUP BY platform
ORDER BY COUNT(age) DESC;


SELECT COUNT(age), platform
FROM  social_media
GROUP BY platform
HAVING COUNT(age) > 5;


SELECT COUNT(age), platform
FROM social_media
GROUP BY platform
HAVING COUNT(age) > 5
ORDER BY COUNT(age) DESC;

--SQL INSERT INTO

INSERT INTO social_media (time_spent, interests , location, profession)
VALUES
('13', 'hiking', 'Kenya', 'Engineer')
('23', 'Farming', 'Uganda', 'Doctor')
('9', 'Travellinh', 'Seychelles', 'Guide');


--SQL JOIN

SELECT social_media.gender, netflix_titles.type, social_media.age
FROM social_media
INNER JOIN netflix_titles ON social_media.Country=netflix_titles.Country;


SELECT social_media.gender, netflix_titles.type, social_media.age
FROM social_media
LEFT JOIN netflix_titles ON social_media.Country=netflix_titles.Country
ORDER BY social_media.age;


SELECT social_media.gender, netflix_titles.type, social_media.age
FROM social_media
RIHT JOIN netflix_titles ON social_media.Country=netflix_titles.Country;



SELECT social_media.gender, netflix_titles.type, social_media.age
FROM social_media
FULL OUTER JOIN netflix_titles ON social_media.Country=netflix_titles.Country
ORDER BY social_media.age;


--To select distinct Countries from both tables

SELECT Country FROM social_media
UNION
SELECT Country FROM netflix_titles
ORDER BY Country;



--To selct from both tables including duplicate countries

SELECT Country FROM social_media
UNION ALL
SELECT Country FROM netflix_titles
ORDER BY Country;


-SQL ORDER BY

SELECT age, Country FROM social_media
WHERE Country='Germany'
UNION
SELECT age, Country FROM netflix_titles
WHERE Country='Germany'
ORDER BY age;


SELECT * INTO social_media
FROM netflix_titles
WHERE Country = 'Germany';



INSERT INTO social_media (age, City, Country)
SELECT age, City, Country FROM netflix_titles
WHERE Country='Australia';




--SQL CASE

SELECT show_id, release_year,
CASE
    WHEN release_year > 2018 THEN 'The quantity is greater than 2018'
    WHEN release_year = 2018 THEN 'The quantity is 2018'
    ELSE 'The release_year is under 2018'
END AS launch year
FROM netflix_titles;


--Using COALESCE() function

SELECT age * (time_spent + COALESCE(income, 0))
FROM Soial_media;


#Using SQl SELECT INT0

SELECT Country INTO social_media
FROM netflix_titles
ORDER BY Country;

--Example 2 of Using SQl SELECT INT0.COPPYING DATA INTO AN EXTERNAL DATAASE
SELECT type,release_year,duration INTO Social_media
FROM netflix_titles
GROUP BY Country;

SELECT * INTO netflix_titlesBackupdata
FROM netflix_titles;

SELECT type, Country INTO netflix_titlesBackupdata
FROM netflix_titles;


--copying data from one table into another in the same database

INSERT INTO Social_media (type, release_year, Country)
SELECT type, release_year, Country FROM netflix_titles
WHERE Country='Germany';

--CREATING/REPLACNG A VEW
CREATE OR REPLACE VIEW [USACustomers] AS
SELECT *FROM netflix_titles
WHERE Country = "United States";

--CREATING A PROCEDURE
CREATE PROCEDURE SelectAllCustomers
AS
SELECT * FROM netflix_titles
GO;

--CREATING A PROCEDURE on MYSQL Workbench
CREATE PROCEDURE SelectAllCustomers()
BEGIN
    SELECT * FROM netflix_titles;
END

DELIMITER ;


--QUERIES

--NESTED QUERIES
SELECT name
FROM employees
WHERE department_id = (SELECT department_id FROM departments
WHERE location_id = (SELECT location_id FROM locations
WHERE city = 'Nairobi'));



-/*Exists Subqueries.Testig existence of roews in a

subquery.uses (EXISTS ,NOT EXISTS)*/

SELECT name 
FROM employees 
WHERE EXISTS(SELECT 1 FROM departments WHERE manager_id 
= employees.employee_id);



--Scalar subqueries.returns a single value(single row and column)

SELECT name,(SELECT MAX(salary) FROM employees)
AS max_salary
FROM employees;


/*Single_row subqueries.Used with (=,>,<,>=,<=),
returns single row of results*/

SELECT name FROM employees WHERE salary = (SELECT MAX(salary)FROM employees);


/*Multiple-column subqueries.Returns more than one column of results.
Used in (FROM, SELECT nad WHERE clauses)*/

SELECT department_id,department_name
FROM departments
WHERE(department_id,location_id) IN 
(SELECT department_id,location_id FROM location);

---On mysql workbench
DELIMITER $$
CREATE DEFINER=`root`@`localhost` PROCEDURE `new_procedure`()
BEGIN SELECT * FROM netflix_titles
LIMIT 3;

END$$
DELIMITER ;


#TRIGGERS IN  MySQL WORKBENCH.(THE before INSERT TRGGER)

create database triggers;
SHOW databases;

use triggers;

#before nsert trigger

create table customers
(cust_id int, age int, name varchar(30));

#delimiter id a marker for an end to each command

delimiter  //
create trigger age_verify before insert on customers
for each row 
if new.age < 0 then set  new.age = 0;
end if; //

insert into customers values
(101,27,'James'),
(102,-40,'Army'),
(103,32,"Ben"),
(104,-39,'Angela');

select *from customers;

#ALL EGATIVE AGES GET CONVERTED INTO 0 AS PER THE TRIGGER AS BELOW

101	27	James
102	0	Army
103	32	Ben
104	0	Angela

##TRIGGERS IN  MySQL WORKBENCH.(THE after  INSERT event TRGGER)
create table customers1(
id int auto_increment primary key,name varchar(40) not null,email varchar(30) ,brthdate date);

create table message (id int auto_increment ,messageid int ,message varchar(300) not null,primary key(id,messageid) );


use triggers;
create table message (id int auto_increment ,messageid int ,message varchar(300) not null,primary key(id,messageid) );
SHOW tables;


DELIMITER //

CREATE TRIGGER check_null_dob 
AFTER INSERT ON customers1 
FOR EACH ROW 
BEGIN
    IF NEW.birthdate IS NULL THEN 
        INSERT INTO message (messageid, message) 
        VALUES (NEW.messageid, 'UpdateDOB');
    END IF;
END //

DELIMITER ;



insert into customers1(name ,email,birthdate)
values('Nancy','nancy@abc.com',null);

[10:05 AM, 8/19/2024] Papa: /*Pivoting and unpivoting,
Pivoting: Convert rows into columns, often for reporting purposes.
Unpivoting: Convert columns back into rows*/

-- Pivot example

SELECT *
FROM (
    SELECT department_id, job_id, salary
    FROM employees
) AS SourceTable
PIVOT (
    SUM(salary)
    FOR job_id IN ([IT_PROG], [SA_REP])
) AS PivotTable;
[10:38 AM, 8/19/2024] Papa: #FOR and IN for SQl
 #  FOR
--FOR UPDATE
/*FOR  keyword is used in  FOR UPDATE and FOR XML/JSON
Used to lock the rows taht are selected by a query ensuring 
that no other transaction can modify /delete these rows until the current
transaction is completed*/


SELECT employee_id, salary
FROM employees
WHERE department_id = 10
FOR UPDATE;

--FOR XML/JSON
/*The FOR XML and FOR JSON clauses are used to 
format the output of a query as XML or JSON */

SELECT employee_id, first_name, last_name
FROM employees
FOR XML AUTO;

SELECT employee_id, first_name, last_name
FROM employees
FOR JSON AUTO;
[11:54 AM, 8/19/2024] Papa: /*Pivoting and unpivoting,
Pivoting: Convert rows into columns, often for reporting purposes.
Unpivoting: Convert columns back into rows*/

-- Pivot example

SELECT *
FROM (
    SELECT department_id, job_id, salary
    FROM employees
) AS SourceTable
PIVOT (
    SUM(salary)
    FOR job_id IN ([IT_PROG], [SA_REP])
) AS PivotTable;


#FOR and IN for SQl
 #  FOR
--FOR UPDATE
/*FOR  keyword is used in  FOR UPDATE and FOR XML/JSON
Used to lock the rows taht are selected by a query ensuring 
that no other transaction can modify /delete these rows until the current
transaction is completed*/


SELECT employee_id, salary
FROM employees
WHERE department_id = 10
FOR UPDATE;

--FOR XML/JSON
/*The FOR XML and FOR JSON clauses are used to 
format the output of a query as XML or JSON */

SELECT employee_id, first_name, last_name
FROM employees
FOR XML AUTO;

SELECT employee_id, first_name, last_name
FROM employees
FOR JSON AUTO;



    #IN
/*Used to specify multiple values in 
a WHERE  clause.Allows you to test where a column value matches 
any value within a geven set*/

--IN with a list
SELECT employee_id, first_name, last_name
FROM employees
WHERE department_id IN (10, 20, 30);

--IN with a subquery
/* Used witha subquery to filter records based
on the results of another query*/

SELECT employee_id, first_name, last_name
FROM employees
WHERE department_id IN (SELECT department_id FROM departments WHERE location_id = 1700);

[12:12 PM, 7/22/2024] Papa: /*Multiple-row subqueries,returns 
more than one row of results.Used with (IN,ANY,ALL)*/

SELECT name FROM employees WHERE department_id
IN (SELECT department_id FROM departments WHERE location_id =35);


/*Single_row subqueries.Used with (=,>,<,>=,<=),
returns single row of results*/

SELECT name FROM employees WHERE salary = (SELECT MAX(salary)FROM employees);

/*Multiple-column subqueries.Returns more than one column of results.
Used in (FROM, SELECT nad WHERE clauses)*/

SELECT department_id,department_name
FROM departments
WHERE(department_id,location_id) IN 
(SELECT department_id,location_id FROM location);


--Scalar subqueries.returns a single value(single row and column)

SELECT name,(SELECT MAX(salary) FROM employees)
AS max_salary
FROM employees;
…
[1:09 PM, 7/22/2024] Papa: --Nested subqueries

SELECT name
FROM employees
WHERE department_id = (SELECT department_id FROM departments
WHERE location_id = (SELECT location_id FROM locations
WHERE city = 'Nairobi'));


-/*Exists Subqueries.Testig existence of roews in a

subquery.uses (EXISTS ,NOT EXISTS)*/

SELECT name 
FROM employees 
WHERE EXISTS(SELECT 1 FROM departments WHERE manager_id 
= employees.employee_id);
[5:36 PM, 7/26/2024] Papa: --The grant statement to table
GRANT SELECT,INSERT ON TABLE Papi TO Tonny;

--The grant statement to Database

GRANT ALL PRIVILEGES ON DATABASE ntflix_series TO Onwonga
WITH GRANT OPTION;
[5:40 PM, 7/26/2024] Papa: --The revoke statement to a table
REVOKE SELECT,INSERT ON TABLE employees FROM Tonny;

--The revoke statement to a database
REVOKE ALL PRIVILEGES ON DATABASE Papito FROM Onwonga

#Difference between IN and OR keywords.


#OR
#Used with WHERE clause

SELECT * FROM Customers
WHERE Country = 'Germany' OR  Country ='Spain';



#IN  
#Specify the WHERE clause.Shorthand for multiple OR operations


SELECT * FROM Customers
WHERE Country IN ('Germany','France','UK');


#NOT operator
#Can be use with BETWEEN,IN ,LIKE,GRETER THAN,NOT LESS THAN,

SELECT * FROM Customers
WHERE NOT country = 'Spain';



SELECT * FROM Customers
WHERE city NOT IN ('Paris','London');

SELECT *FROM Customers 
WHERE NOT customerID>45;

SELECT *FROM Customers
WHERE CustomerID NOT BETWEEN 10 AND 60;

[1:24 PM, 9/11/2024] Papa: #CONSTRAINTS

#1.NOT NULL

CRETE TABLE Papi(
PapiID INT AUTO_INCREMENT PRIMARY KEY,
Firstname VARCHAR(50) NOT NULL,
Lastname VARCHAR(50) NOT NULL
);


#2.UNIQUE-Ensures that all the values in the column are different

CREATE TABLE Emperor(
EmployeeID INT AUTO_INCREMENT PRIMARY KEY,
Email VARCHAR(100)UNIQUE
);


#3.PRIMARY KEY-this uniquely identifies eacgh record in a table

CREATE TABLE Employees(
ID INT AUTO_INCREMENT PRIMARY KEY,
Firstname VARCHAR(50));

#4.FOREIGN KEY-its a value that exists in another table

CREATE TABLE Orders(
ID INT PRIMARY KEY,
Salary id,
FOREIGN KEY(EmployeeID) REFERENCES Employees(EmployeesID));

#5.CHECK-Ensures all values in a column 


















#CONSTRAINTS

#1.NOT NULL

CRETE TABLE Papi(
PapiID INT AUTO_INCREMENT PRIMAR…
[3:25 PM, 9/11/2024] Papa: #5.CHECK-Ensures all values in a column

CREATE TABLE Omotangi(
ID INT AUTO_INCREMENT PRIMARY KEY,
Salary DECIMA(10,2) CHECK(salary > 0)
);
[3:46 PM, 9/11/2024] Papa: #6.DEFAULT-It sets a default value for a column when no value has been provided


CREATE TABLE Onwonga(
ID INT AUTO_INCREMENT PRIMARY KEY,
Normaldate DATE DEFAULT CURRENT_DATE
);
[9:49 AM, 9/12/2024] Papa: CREATE TABLE Onwonga(
ID INT AUTO_INCREMENT PRIMARY KEY,
Normaldate DATE DEFAULT CURRENT_DATE
);
[10:25 AM, 9/12/2024] Papa: #Difference between IN and OR keywords.


#OR
#Used with WHERE clause

SELECT * FROM Customers
WHERE Country = 'Germany' OR  Country ='Spain';

[12:55 PM, 8/26/2024] Papa: #MYSQL FUNCTIONS

   #STRING FUNCTIONS

SELECT CONCAT('Hello','''world') AS greeting;

SELECT LENGTH ('Hello world') AS lenght;

SELECT LOWER('hello');

#SUBSTRING(string,start_position,length)

SELECT SUBSTRING ('Hello world',7,5) AS substring;

#REPLACE (string,old_string,new_string)
SELECT REPLACE ('Hello world','world','mysql')
---RESULT; HELLO MYSQL
[12:26 PM, 9/3/2024] Papa: #NUMERIC FUNCTIONS

#  ROUND()
SELECT ROUND(10.432,1);
--Answer  10.5

#  SQRT()
SELECT SQRT(16);

---Answer = 4

# MOD Returns the remainder of a division operation

SELECT MOD(10,3);
--Result 1

# DATE AND TIME TRANSACTIONS

# NOW() -returns the current date and time.

SELECT NOW();

#CURDATE() and CURTIME() wreturns tghe current date
or time

SELECT CURDATE();
[1:13 PM, 9/4/2024] Papa: #CONTROL FLOW FUNCTIONS

#IF -returns a value if a condition is met.

SELECT IF(salary>5000 ,'High','Low') FROM employees;

#COALESCE-returns the first non-Null value in a list

SELECT COALESCE(NULL,NULL,'Hello');

--result-Hello


#JSON functions
[11:25 AM, 9/11/2024] Papa: #DROP
DROP TABLE Papi;
DROP DATABASE Onwonga;

#BACKUP

BACKUP DATABASE Onwonga TO DISK = 'C:\Documents';


#TRUNCATE

TRUNCATE TABLE Papi;


#ALTER

ALTER TABLE Papi
ADD email VARCHAR

ALTER TABLE Papi
DROP COLUMN email

#using ROLLUP together with  GROUP BY

SELECT
   customer_id,
   SUM(quantity * unit_price) amount
FROM
   orders
INNER JOIN order_items USING (order_id)
WHERE
   status      = 'Shipped' AND 
   salesman_id IS NOT NULL AND 
   EXTRACT(YEAR FROM order_date) = 2017
GROUP BY
   ROLLUP(customer_id);


SELECT region,
       product_category,
       SUM(sales_amount) AS total_sales
FROM sales
GROUP BY ROLLUP (region, product_category)
ORDER BY region, product_category;


#ORACLE EXTRACT
#The Oracle EXTRACT() function extracts a specific component (year, month, day, hour, minute, second, etc.,) 
from a datetime or an interval value

SELECT
  EXTRACT( HOUR FROM INTERVAL '5 04:30:20.11' DAY TO SECOND )
FROM
  dual;
