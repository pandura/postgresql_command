---postgress shell => connecto to server on window \
- server local or url \
atabase \
port \
username \
password \

\q => quit from shell \
help \
\l => list of ddatabase \
\
\
---create database: \
CREATE DATABASE name; => importent semicolen \
\l => list of databases \
\
---connect to concreat database \
psql --help => show commands \
psql -h localhost -p 5432 -U postgres test => localhost/url to database \
 \
\c <database name> => connect to database, switch between database \
\
DROP DATABASE => delete database \

---CREATE USER with rights \
--exist user \
ALTER USER user_name CREATEDB; \
ALTER USER user_name CREATEDB CREATEROLE LOGIN; \
--new user \
CREATE ROLE user_name PASSWORD 'tYPe_YoUr_PaSSwOrD' NOSUPERUSER CREATEDB CREATEROLE INHERIT LOGIN; \
sudo -u postgres createuser --createdb --createrole --pwprompt user_name \

NOSUPERUSER - the user being created does not have superuser rights (like the postgres user, recommended). \
CREATEDB - the user being created can create databases which it will own. \
CREATEROLE - the user being created can create roles (logins) for objects it owns or has specific access to (like databases it has created). \
INHERIT - the user being created inherits some default options, optional. \
LOGIN - the user being created has the right to log in. \
\
\du => list of owners with roles \
DROP USER -name- => delete user Data Database of user should be delete \
\
---CREATE TABLE IN DATABASE \
-- create table without constrains =>CREATE TABLE -name_table- (column name + data type + constrains if any) => 
	example: CREATE TABLE person (id int, 
					first_name VARCHAR(50), 
					last_anem VARCHAR(50), 
					gender VARCHAR(6), 
					data_of_birth TIMESTAMP/DATE); \
-example type => int => number, VARCHAR(40) => 40 character string/			link https://www.postgresql.org/docs/current/datatype.html \
	when we use "(" and enter we can write in next line CREATE TABLE person (
										id INT,
										first_Name VARCHAR(50),...); \
\d deeper \d -table name- check list of table in database \
\dt table list \
	\
-- create table with constrains => \ 
	example CREATE TABLE person (id BIGSERIAL NOT NULL PRIMARY KEY, 
					first_name VARCHAR(50) NOT NULL, 
					last_anem VARCHAR(50) NOT NULL, 
					gender VARCHAR(6) NOT NULL, 
					data_of_birth TIMESTAMP/DATE NOT NULL,
					email VARCHAR(150)) \
	type BIGSERIAL generate sequence in table \
	/
DROP CONSTRAINT => example: ALTER TABLE person DROP CONSTRAINT person_pkey; (person_pkey => generate when primary key)//

DROP TABLE -name- => delete table \
\
---Insert Into table \
--INSERT INTO -tablename-(columns1, columns2, coluns3, ...) VALUES (value1, value2, value3, ...); \
	example => INSERT INTO person (first_name, last_name, gender, data_of_birth) VALUES ('Grzegorz', 'Dura', 'Male', Date '1981-01-01' ); => posible is in 2 lines \
		INSERT INTO person (first_name, last_name, gender, data_of_birth) <enter>
			VALUES ('Grzegorz', 'Dura', 'Male', Date '1981-01-01' ); => possible multiple data VALUES ('Grzegorz', 'Dura', 'Male', Date '1981-01-01' ), ('Grzegorz', 'Dura', 'Male', Date '1981-01-01' ); \

/
--DELETE ROW IN TABLE /
DELETE FROM -table_name- WHERE -condition- /
example => DELETE FROM person WHERE id=1;

check tables SELECT * FROM person \
\
INSERT INTO person(email) VALUES ('test') => possible if other column dont have null \
\
--DROP TABKE -talbe name-; \

DATA GENERATOR => mockaroo => https://www.mockaroo.com/ => file generator \
"\i" -path- example wsl in widnow =>  /mnt/c/Users/48606/Downloads/person.sql  => download file to postgresql  \

\
---SELECT -column- FROM -table name- \
--ORDER BY \
	SELECT -column- FROM -Table name- ORDER BY -column name- ASC/DESC	\
--DELETE DUPLICATE \
- DISTINCT - unique value of column \
	SELECT DISTINCT -column_name FROM -Table_name ORDER BY -column_name ASC/DESC \
--WHERE condition \
	SELECT -column_name- FROM -Table_name- WHERE -name_column- = '-string-' \
-Anather condition \
	SELECT -column- FROM -Table_name- WHERE -name_column- = '-string-' AND / OR -name_column- = 'string/ number' \
	SELECT -column- FROM -Table_name- WHERE -name_column- = '-string-' AND / OR (-name_column- = 'string/ number' OR -name_column- = '-string-/ number') \
---Comparison \
-- SELECT 1 = 1; true \
-- SELECT 1 > 0; true \
-- SELECT 1 >= 2; true \
-- SELECT 1 <> 2; true => <> NOT \

--- LIMIT \
	SELECT -column- FROM -table_name- LIMIT 5; \
--- OFFSET - exclude \
	SELECT -column- FROM -table_name- OFFSET 5; \
	SELECT -column- FROM -table_name- OFFSET 5 LIMIT 5; => exclude first 5 then limit to 5 \
--- FETCH => limit/exclude => use seldom \
	SELECT -column- FROM -table_name- OFFSET -Number- FETCH FIRST 5 ROW ONLY; \
	OFFSET start { ROW | ROWS } FETCH { FIRST | NEXT } [ row_count ] { ROW | ROWS } ONLY \
--- IN => array of values \

	SELECT -column- FROM -table_name- WHERE -name_column- = '-string-' OR -name_column- = 'string/ number' \
	SELECT -column- FROM -table_name- WHERE -name_column- IN ('-value_from_column-', '-value_column-', '-value_from_column-') \
\
--- BETWEEN \
	SELECT -column- FROM -Table_name- WHERE -name_of_column- BETWEEN DATE '2001-01-01' AND '2022-01-01' \
\
--- LIKE \
	SELECT -column- FROM -table_name- WHERE -name_of_column- LIKE ('-pattern-') \
	example => SELECT * FROM person WHERE email LIKE ('%@gmail.com'); => % any character \
	example => SELECT * FROM person WHERE email LIKE ('%@gmail.%'); => % any character \
	example => SELECT * FROM person WHERE email LIKE '______'; => _ concreate quantity of character in this case 6 \
	example => SELECT * FROM person WHERE email LIKE ('%@gmail.__'); => _ concreate quantity of character in this case 2 \
--- ILIKE => like LIKE case sensenitive \

--- GROUP BY => agregate data base of column \
	SELECT -column- function() FROM -table_name- GROUP BY -column- \
	example => SELECT country_of_birth COUNT(*) FROM person GROUP BY country_of_birth \

	function of agregate https://www.postgresql.org/docs/9.5/functions-aggregate.html \

--- GROUP BY... HAVING => GROUP BY WITH CONDITION \
	SELECT -column- function(*) FROM -Table_name- GROUP BY -column- HAVING COUNT(*) > 5 \
	example => SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth HAVING COUNT(*) > 5 \

---Agregation \

--COUNT function - quantity of element - no null \
	SELECT COUNT(*) FROM -table_name- \
	SELECT email, COUNT(*) FROM person GROUP BY email;- count email by grouping \
	example => SELECT job_id,COUNT(*) AS "Number of employees" \
	FROM employees \
	WHERE salary<12000 \
	GROUP BY job_id \
	HAVING COUNT(*)>=5 \
	ORDER BY COUNT(*) DESC;
 \ \
-- MAX() \
	SELECT MAX(-column-) FROM -table_name-; \
	example => SELECT MAX(price) FROM car; \
-- MIN() \
	SELECT MIN(-column-) FROM -table_name-; \
	example => SELECT MIN(price) FROM car; \
-- AVG() \
	SELECT AVG(-column-) FROM -table_name-; \
	example => SELECT AVG(price) FROM car; \
-- SUM() \
	SELECT SUM(-column-) FROM -table_name-; \
	example => SELECT SUM(price) FROM car; \
-- ROUND(AVG()) \
	SELECT ROUND(AVG(-column-), decimal) FROM -table_name-; \
	example => SELECT ROUND(AVG(price), 2) FROM car; \
--Agregation with GROUP BY \
-- MAX() \
	SELECT -column_name-, ... MAX(-column-) FROM -table_name- GROUP BY -column_name-; \
	example => SELECT make, model,  MAX(price) FROM car GROUP BY make, model; \
\
---ARTHIMETIC OPERATORS - ROUND \

	SELECT price price * 0.10 FROM car; => add additonal column ?column? with 10% price \
	SELECT price ROUND(price * 0.10) FROM car; \
	
	SELECT price ROUND(price * 0.10), ROUND(price - ROUND(price * 0.1, 2), 2) FROM car; \
\
---ALIAS \
	SELECT price price * 0.10 AS -alias-FROM car; \
	example SELECT price ROUND(price * 0.10) AS <alias>, ROUND(price - ROUND(price * 0.1, 2), 2) AS <alias> FROM car; \
\
---COALESCE => default value if is not present \

	example => SELECT COALESCE(null, 1) => if null default 1 \
	example => SELECT COALESCE(null, null, 1) if first value is not present try secound \
	
	example => SELECT COALESCE(-colum_nema-, 1) FROM -table_name- => if column name === null ? 1 \
		SELECT COALESCE(email, "Email not provided") FROM person \

---NULLIF => if first argument is not equel second return first if is will be null => help secure error in devision by 0 \
	example => SELECT NULLIF(1, 10); result 1 \
	example SELECT 10/ NULLIF(0, 0) => null not error \
	example SELECT COALESCE(10 / NULLIF(0, 0), 0); \
\
---Timestamps => https://www.postgresql.org/docs/current/datatype-datetime.html \
	example => SELECT NOW() \
	example => SELECT NOW()::DATE; y-m-d \
	example => SELECT NOW()::TIME; h:m:s \
--Subtracting dates => add or minus date \
	example => SELECT NOW() -/+  INTERVAL '1 YEAR'; \
	example => SELECT NOW() -/+  INTERVAL '1 MONTHS'; \
	example => SELECT NOW() -/+  INTERVAL '1 DAYS'; \
	example => SELECT NOW()::DATE -/+  INTERVAL '1 DAYS'; \
	example => SELECT (NOW() -/+  INTERVAL '1 DAYS')::DATE; \
--Extract time => separetl from date \
	SELECT EXTRACT(YEAR/MONTH/DAYS FROM NOW()) \ \

--AGE FUNCTION => calculate from now\
	SELECT -column_name- AGE(NOW(), -column_with_type_DATE-) FROM -data_name- \
	exampe => SELECT geneder, first_name, last_na,e, AGE(NOW(), date_of_birth) FROM person \
	exampe with extraction => SELECT geneder, first_name, last_na,e, EXTRACT(MONTH FROM AGE(NOW(), date_of_birth)) FROM person \
\
---PrimaryKEY - value wich uniquerly identify a record row in table \
DROP CONSTRAINT / example => ALTER TABLE person DROP CONSTRAINT person_pkey; (person_pkey => generate when primary key)\ \
ADDING PRIMARY KEY to existed table \
ALTER TABLE person ADD PRIMARY KEY (id); => possible to add multiple values like id => id must by unique \ 
\


--- Unique Constraints - help have uniqe value per column example email - error when add this same value \
	- FIRST METHOD => ALTER TABLE -table_name- ADD CONSTRAINT -name- UNIQUE(-name_of_column-)/UNIQUE(-name_of_column-, -name_of_column-, ...) \
	- SECOUND METHOD => ALTER TABLE -table_name- ADD UNIQUE (-column name-);
	example => ALTER TABLE person ADD CONSTRAINT unique_email_address UNIQUE(email) \
--- Drop unique Constraint \
	ALTER TABLE -table_name- DROP CONSTRAINT -name- \
	example => ALTER TABLE person DROP CONSTRAINT unique_email_address; \
--- CHECK CONSTRAINS - generate constrains on base CHECK if condition is false then error \
	ALTER TABLE -table_name- ADD CONSTRAINT -name- CHECK (condition)\
	example: ALTER TABLE person ADD CONSTRAINT gender_constraint CHECK (gender = "F" OR gender = "M");
\
	

\
---UPDATE \
UPDATE -table_name- SET -column_name- = 'value' WHERE ... \
example UPDATE person SET email = 'test@test.pl' WHERE id =1; \
update multicolumn: UPDATE -table_name- SET -column_name- = 'value', -column_name- = 'value' WHERE ...
\ 
\
---ADD COLUMN TO TABLE
ALTER TABLE -table_name- ADD -new_column- TYPE\
example ALTER TABLE person ADD dupa VARCHAR(20);
\
--DROP COLUMN OF TABLE\
ALTER TABLE person DROP COLUMN dupa;\
--RENAMTE COLUMN \
ALTER TABLE person RENAME COLUMN dupa TO column_1;\



\
---DELETE record\
DELETE FROM -table-name- => delete all row\
DELETE FROM person WHERE ...; \
example: DELETE FROM person WHERE id = 1; \
example: DELETE FROM person WHERE id = 1 AND country_of_birth = 'Poland'; \
\
\
---ON CONFLICT - handle conflict when column has UNIQUE value other ways ON CONFLICT doesent function /
--DO NOTHING - INSERT INTO (-name_column-, ...) VALUES (-values-) ON CONFLICT (-potential_name_column_conflict-) DO NOTHING;\
example => INSERT INTO person (id, first_name, last_name, email, gender, data_of_birth, country_of_birth) VALUES (9999, 'GRZEGORZ', 'DURA', 'test141@sun1.com', 'F', '1981-03-23', 'POLAND') ON CONFLICT (email) DO NOTHING;\
--DO UPDATE \ 
example => INSERT INTO person (id, first_name, last_name, email, gender, data_of_birth, country_of_birth) VALUES (9999, 'GRZEGORZ', 'DURA', 'test1411@sun1.com', 'F', '1981-03-23', 'POLAND') ON CONFLICT DO UPDATE SET first_name = EXCLUDED.first_name, email = EXCLUDED.email;\
\
\
---FOREIGN KEYS => column which reference to primary key in another table. This column has value type this same like primary key in anather table. Foreign Keys is UNIQUE
--BUILD RELATIONSIP => two table in database\
	ONE person can have only one car (UNIQUE(car_id))\
CREATE TABLE person (
	id BIGSERIAL NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	email VARCHAR(150),
	gender VARCHAR(7) NOT NULL,
	data_of_birth DATE NOT NULL,
	country_of_birth VARCHAR(50) NOT NULL,
	car_id BIGINT REFERENCES car (id),
	UNIQUE(car_id)

);\
CREATE TABLE car ( 
id BIGSERIAL NOT NULL PRIMARY KEY, 
make VARCHAR(100) NOT NULL, 
model VARCHAR(100) NOT NULL, 
price NUMERIC(19, 2) NOT NULL
);  \
\
--Update Foreign Keys\
UPDATE -table_name- SET -column_name- = 'value' WHERE ... \
example UPDATE person SET car_id = 1 WHERE id = 2;/ 
\
--DELETE records with foreign keys records which has foreign key which is use in other table we can't delete with method DELETE FROM car WHERE id = 111; To delete we mast first delete record where foreign key is used or update this record \

\
---INNER JOINS => method of combaine two tables, method take all common records not this which don't have FOREIGN KEYS. Importent si Foreign keys, one table direct to second \
SELECT * FROM -one_table_name- JOIN -two_table_name- ON -one_table_name-.forein_key = -two_table_name-.id \
example => SELECT * FROM person JOIN car ON person.car_id = car.id; \
\ 
\
to show in better view \x => expanded display on, onother \d dispaly of \ 
\
to show column what we want SELECT -one_table_name-.column1, -one_table_name-.column2, -two_table_name-.column1, -two_table_name-.column2 FROM -one_table_name- JOIN -two_table_name- ON -one_table_name-.forein_key = -two_table_name-.id
example SELECT person.fist_name, car.make, car.model, car.price FROM -one_table_name- JOIN -two_table_name- ON -one_table_name-.forein_key = -two_table_name-.id
/
---LEFT JOINS =>method of combaine two tables, method take all recods from left table (event don't match) and common records . Importent is Foreign keys, one table direct to second \
SELECT * FROM -one_table_name- LEFT JOIN -two_table_name- ON -one_table_name-.forein_key = -two_table_name-.id \
example => SELECT * FROM person LEFT JOIN car ON person.car_id = car.id; => get also people who don't have car \
 SELECT * FROM person LEFT JOIN car ON person.car_id = car.id WHERE car.* IS NULL; => get all people who don't have car \
SELECT * FROM person WHERE car_id IS NULL
\
value IS NULL => return TRUE, value = NULL return false
\
-one_table_name-.* => means all tables
\
\
---GENERATE csv\
\copy (SELECT * FROM -table name- ...) TO -destination/filename.csv- DELIMITER ',' CSV HEADER \
example => \copy (SELECT * FROM person LEFT JOIN car ON person.car_id = car.id) TO '/mnt/c/Users/48606/Desktop/testowanie/result' DELIMITER ',' CSV HEADER;
\
\
---Serial & Sequences => BIGSERIAL type => generate new sequence/table which has "last_value", "log_cnt", 'is_called'. Table with BIGSERIAL has column default with function nextval('-table_name-'::regclass) wich increase value in last_value \
example => nextval('person_id_seq'::regclass) is default when create BIGSERIAL => change value related toperson_id_seq
--restart last value in table person_id_seq which was generate with BIGSERIAL \
ALTER SEQUENCE -table_name_id_seq- RESTART WITH -value- \ 
example => ALTER SEQUENCE person_id_seq RESTART WITH 10 \
\
--EXTENSION \
SELECT * FROM pg_available_extensions; => list of available extensios \
example extension => uuid-ossp => install CREATE EXTESION IF NOT EXISTS "uuid-ossp" \
using extension \
\df => list of function connection to installed extension \
SELECT -function_name- => example => SELECT uuid_generate_v4(); \
--using uuid as Primary key => importent UUID type in id \ 
example => create table car ( \
	car_uid UUID NOT NULL PRIMARY KEY, \
	make VARCHAR(100) NOT NULL, \
	model VARCHAR(100) NOT NULL, \
	price NUMERIC(19, 2) NOT NULL \
);\
table car must be first becouse person table has references to car, other hend could be problem with relation and person will not generate
\
create table person ( \
	person_uid UUID NOT NULL PRIMARY KEY, \
	first_name VARCHAR(50) NOT NULL, \
	last_name VARCHAR(50) NOT NULL, \
	email VARCHAR(150), \
	gender VARCHAR(7) NOT NULL, \
	data_of_birth DATE NOT NULL, \
	country_of_birth VARCHAR(50), \
	car_uid UUID REFERENCES car(car_uid), \
	UNIQUE(car_uid), \
	UNIQUE(email) \
); \
insert into person (person_uid, first_name, last_name, email, gender, data_of_birth, country_of_birth) values (uuid_generate_v4(),'Daniele', 'Luxmoore', 'dluxmoore0@ow.ly', 'F', '2022-06-30', 'Morocco'); \
insert into car (car_uid, make, model, price) values (uuid_generate_v4(), 'Chevrolet', 'Silverado 1500', 68947.35);\
\
example => SELECT * FROM person JOIN car ON person.car_uid = car.car_uid; => importent if the car_uid is using in two table (foreign key and primary key) we can write shorten SELECT * FROM person JOIN car USING (car_uid);  \

what next => https://www.youtube.com/watch?v=ldYcgPKEZC8&t=1129s


\
\
\
linux command \ 
-pwd path to current folder
/mnt/c/Users/48606/Desktop/testowanie     
