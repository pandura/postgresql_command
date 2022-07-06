---postgress shell => connecto to server on window
- server local or url
atabase
port
username
password

\q => quit from shell
help
\l => list of ddatabase


---create database:
CREATE DATABASE name; => importent semicolen
\l => list of databases

---connect to concreat database
tsql --help => show commands
psql -h localhost -p 5432 -U postgres test => localhost/url to database

\c <database name> => connect to database, switch between database

DROP DATABASE => delete database

---CREATE USER with rights
--exist user
ALTER USER user_name CREATEDB;
ALTER USER user_name CREATEDB CREATEROLE LOGIN;
--new user
CREATE ROLE user_name PASSWORD 'tYPe_YoUr_PaSSwOrD' NOSUPERUSER CREATEDB CREATEROLE INHERIT LOGIN;
sudo -u postgres createuser --createdb --createrole --pwprompt user_name

NOSUPERUSER - the user being created does not have superuser rights (like the postgres user, recommended).
CREATEDB - the user being created can create databases which it will own.
CREATEROLE - the user being created can create roles (logins) for objects it owns or has specific access to (like databases it has created).
INHERIT - the user being created inherits some default options, optional.
LOGIN - the user being created has the right to log in.

\du => list of owners with roles
DROP USER <name> => delete user Data Database of user should be delete

---CREATE TABLE IN DATABASE
-- create table without constrains =>CREATE TABLE <name_table> (column name + data type + constrains if any) => 
	example: CREATE TABLE person (id int, 
					first_name VARCHAR(50), 
					last_anem VARCHAR(50), 
					gender VARCHAR(6), 
					data_of_birth TIMESTAMP/DATE)
-example type => int => number, VARCHAR(40) => 40 character string			link https://www.postgresql.org/docs/current/datatype.html
	when we use "(" and enter we can write in next line CREATE TABLE person (
										id INT,
										first_Name VARCHAR(50),...)
\d deeper \d <table name> check list of table in database 
\dt table list
	
-- create table with constrains => 
	example CREATE TABLE person (id BIGSERIAL NOT NULL PRIMARY KEY, 
					first_name VARCHAR(50) NOT NULL, 
					last_anem VARCHAR(50) NOT NULL, 
					gender VARCHAR(6) NOT NULL, 
					data_of_birth TIMESTAMP/DATE NOT NULL,
					email VARCHAR(150))
	type BIGSERIAL generate sequence in table
DROP TABLE <name> => delete table

---Insert Into table
--INSERT INTO <tablename>(columns1, columns2, coluns3, ...) VALUES (value1, value2, value3, ...)
	example => INSERT INTO person (first_name, last_name, gender, data_of_birth) VALUES ('Grzegorz', 'Dura', 'Male', Date '1981-01-01' ); => posible is in 2 lines 
		INSERT INTO person (first_name, last_name, gender, data_of_birth) <enter>
			VALUES ('Grzegorz', 'Dura', 'Male', Date '1981-01-01' ); => possible multiple data VALUES ('Grzegorz', 'Dura', 'Male', Date '1981-01-01' ), ('Grzegorz', 'Dura', 'Male', Date '1981-01-01' )



check tables SELECT * FROM person

INSERT INTO person(email) VALUES ('test') => possible if other column dont have null

--DROPT TABKE <talbe name>;

DATA GENERATOR => mockaroo => https://www.mockaroo.com/ => file generator
"\i" <path> example wsl in widnow =>  /mnt/c/Users/48606/Downloads/person.sql  => download file to postgresql  


---SELECT <column> FROM <table name>
--ORDER BY
	SELECT <column> FROM <Table name> ORDER BY <column name> ASC/DESC	
--DELETE DUPLICATE
- DISTINCT - unique value of column
	SELECT DISTINCT <column> FROM <Table name> ORDER BY <column name> ASC/DESC
--WHERE condition
	SELECT <column> FROM <Table name> WHERE <name of column> = '<string>' 
-Anather condition
	SELECT <column> FROM <Table name> WHERE <name of column> = '<string>' AND / OR <name of column> = '<string>/ number'
	SELECT <column> FROM <Table name> WHERE <name of column> = '<string>' AND / OR (<name of column> = '<string>/ number' OR <name of column> = '<string>/ number')
---Comparison
-- SELECT 1 = 1; true
-- SELECT 1 > 0; true
-- SELECT 1 >= 2; true
-- SELECT 1 <> 2; true => <> NOT

--- LIMIT
	SELECT <column> FROM <table name> LIMIT 5;
--- OFFSET - exclude
	SELECT <column> FROM <table name> OFFSET 5;
	SELECT <column> FROM <table name> OFFSET 5 LIMIT 5; => exclude first 5 then limit to 5
--- FETCH => limit/exclude => use seldom
	SELECT <column> FROM <table name> OFFSET <Number> FETCH FIRST 5 ROW ONLY;
	OFFSET start { ROW | ROWS } FETCH { FIRST | NEXT } [ row_count ] { ROW | ROWS } ONLY
--- IN => array of values

	SELECT <column> FROM <Table name> WHERE <name of column> = '<string>' OR <name of column> = '<string>/ number'
	SELECT <column> FROM <Table name> WHERE <name of column> IN ('<value from column>', '<value from column>', '<value from column>')

--- BETWEEN
	SELECT <column> FROM <Table name> WHERE <name of column> BETWEEN DATE '2001-01-01' AND '2022-01-01'

--- LIKE
	SELECT <column> FROM <Table name> WHERE <name of column> LIKE ('<pattern>')
	example => SELECT * FROM person WHERE email LIKE ('%@gmail.com'); => % any character
	example => SELECT * FROM person WHERE email LIKE ('%@gmail.%'); => % any character
	example => SELECT * FROM person WHERE email LIKE '______'; => _ concreate quantity of character in this case 6
	example => SELECT * FROM person WHERE email LIKE ('%@gmail.__'); => _ concreate quantity of character in this case 2
--- ILIKE => like LIKE case sensenitive

--- GROUP BY => agregate data base of column
	SELECT <column> function() FROM <Table name> GROUP BY <column>
	example => SELECT country_of_birth COUNT(*) FROM person GROUP BY country_of_birth

	function of agregate https://www.postgresql.org/docs/9.5/functions-aggregate.html

--- GROUP BY... HAVING => GROUP BY WITH CONDITION
	SELECT <column> function(*) FROM <Table name> GROUP BY <column> HAVING COUNT(*) > 5
	example => SELECT country_of_birth COUNT(*) FROM person GROUP BY country_of_birth HAVING COUNT(*) > 5

---Agregation
-- MAX()
	SELECT MAX(<column>) FROM <table name>;
	example => SELECT MAX(price) FROM car;
-- MIN()
	SELECT MIN(<column>) FROM <table name>;
	example => SELECT MIN(price) FROM car;
-- AVG()
	SELECT AVG(<column>) FROM <table name>;
	example => SELECT AVG(price) FROM car;
-- SUM()
	SELECT SUM(<column>) FROM <table name>;
	example => SELECT SUM(price) FROM car;
-- ROUND(AVG())
	SELECT ROUND(AVG(<column>), decimal) FROM <table name>;
	example => SELECT ROUND(AVG(price), 2) FROM car;
--Agregation wit GROUP BY
-- MAX()
	SELECT <column_name>, ... MAX(<column>) FROM <table name> GROUP BY <column_name>;
	example => SELECT make, model,  MAX(price) FROM car GROUP BY make, model;

---ARTHIMETIC OPERATORS - ROUND

	SELECT price price * 0.10 FROM car; => add additonal column ?column? with 10% price
	SELECT price ROUND(price * 0.10) FROM car;
	
	SELECT price ROUND(price * 0.10), ROUND(price - ROUND(price * 0.1, 2), 2) FROM car;

---ALIAS
	SELECT price price * 0.10 AS <alias>FROM car;
	example SELECT price ROUND(price * 0.10) AS <alias>, ROUND(price - ROUND(price * 0.1, 2), 2) AS <alias> FROM car;

---COALESCE => default value if is not present

	example => SELECT COALESCE(null, 1) => if null default 1
	example => SELECT COALESCE(null, null, 1) if first value is not present try secound
	
	example => SELECT COALESCE(<colum nema>, 1) FROM <Table name> => if column name === null ? 1
		SELECT COALESCE(email, "Email not provided") FROM person

---NULLIF => if first argument is not equel second return first if is will be null => help secure error in devision by 0
	example => SELECT NULLIF(1, 10); result 1
	example SELECT 10/ NULLIF(0, 0) => null not error
	example SELECT COALESCE(10 / NULLIF(0, 0), 0);

---Timestamps => https://www.postgresql.org/docs/current/datatype-datetime.html
	example => SELECT NOW()
	example => SELECT NOW()::DATE; y-m-d
	example => SELECT NOW()::TIME; h:m:s



what next => https://www.youtube.com/watch?v=ldYcgPKEZC8&t=1129s



linux command 
-pwd path to current folder
