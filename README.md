# MySQL Tutorial 

The Goal of this task is to help you familiarise yourself with some CRUD operation using MySQL
by the end of this task you should be able to:
1. Assign a database adminstrator to a database 
2. Grant Privileges
3. Populate a database
4. Perform queries on the database 
5. Join Tables
6. Use the Aggregate Function

## Download MySQL at mysql.com 
* Downloads 
* MySQL Community (GPL) Downloads
* MySQL Community Server
## Windows
* Go to download page
* Windows (x64,64 bit) MSI installer

## Troubleshooting windows installation 
Some computers/laptops might not be able to download 
MySQL because their system might not be update
additonally click on this download link to download the required dependency
https://aka.ms/vs/17/release/vc_redist.x64.exe

## MacOS
* Select first option
## MySQL Locations
* Mac             */usr/local/mysql/bin*
* Windows         */Program Files/MySQL/MySQL _version_/bin*
* Xampp           */xampp/mysql/bin*


## Add mysql to your PATH (MacOS)

```bash
# Current Session
export PATH=${PATH}:/usr/local/mysql/bin
# Permanantly
echo 'export PATH="/usr/local/mysql/bin:$PATH"' >> ~/.bash_profile

```

## Add mysql to your PATH (Windows)

* Go This PC
* Select Windows (C:)
* Select Program Files
* Select MySQL
* Select MySQL Server 8.0.29
* Select bin
* Copy the directory path 
  C:\Program Files\MySQL\MySQL Server 8.0.29\bin
* Right click “This PC”
* Click “Properties”
* Select “Advanced”
* Click “Environment Variables”
* Locate “System variables”
* Click "New”
* Type “Path” in the first entry 
* Paste the directory path you copied in the second entry
* Select "Ok"


## Opening Command Line
Open the Application Command Line. You can find it by searching CMD on the search bar at the bottom left hand corner of your screen (windows 10) alternatively you can find it by searching CMD using the search icon on your task bar (windows 11)

## Verify MySQL Installation
In the command line type

```bash
mysql -- version
```
you shoudld receive output similar to this
```bash
mysql  Ver 8.0.29 for macos12 on x86_64 (MySQL Community Server - GPL)
```
if you receive 
```bash
not recognized as an internal or external command
```
please revisit the installation process and try installing MySQL again

## Login

If verification was successful login by typing
```bash
mysql -u root -p
```

## Show Users

```sql
SELECT User, Host FROM mysql.user;
```

## Create User

```sql
CREATE USER 'someuser'@'localhost' IDENTIFIED BY 'somepassword';
```

## Grant All Priveleges On All Databases

```sql
GRANT ALL PRIVILEGES ON * . * TO 'someuser'@'localhost';
```
```sql
FLUSH PRIVILEGES;
```

## Show Grants

```sql
SHOW GRANTS FOR 'someuser'@'localhost';
```

## Remove Grants

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'someuser'@'localhost';
```

## Delete User

```sql
DROP USER 'someuser'@'localhost';
```

## Exit

```sql
exit;
```

## Login With The New User You Created
```bash
mysql -u someuser -p
```

## Show Databases

```sql
SHOW DATABASES
```

## Create Database

```sql
CREATE DATABASE remote;
```

## Delete Database

```sql
DROP DATABASE remote;
```

## Select Database

```sql
USE remote;
```

## Create Table

```sql
CREATE TABLE users(
id INT AUTO_INCREMENT,
   first_name VARCHAR(100),
   last_name VARCHAR(100),
   email VARCHAR(50),
   password VARCHAR(20),
   location VARCHAR(100),
   dept VARCHAR(100),
   is_admin TINYINT(1),
   register_date DATETIME,
   PRIMARY KEY(id)
);
```

## Delete / Drop Table

```sql
DROP TABLE tablename;
```

## Show Tables

```sql
SHOW TABLES;
```

## Insert Row / Record

```sql
INSERT INTO users (first_name, last_name, email, password, location, dept, is_admin, register_date) values ('Theo', 'Mashego', 'Theo@gmail.com', '12345678','Mbombela', 'Software', 1, now());
```

## Insert Multiple Rows

```sql
INSERT INTO users (first_name, last_name, email, password, location, dept,  is_admin, register_date) values ('Thabo', 'Zulu', 'Thabo@gmail.com', '123456', 'Johannesburg', 'Graphic', 0, now()), ('Lerato', 'Ndlovu', 'Lerato@gmail.com', '123456', 'Mbombela', 'Software', 0, now()),('Nyasha', 'Maschette', 'Nyasha@gmail.com', '123456', 'Mbombela', 'software', 1, now());
```

## Select

```sql
SELECT * FROM users;
SELECT first_name, last_name FROM users;
```

## Where Clause

```sql
SELECT * FROM users WHERE location='Massachusetts';
SELECT * FROM users WHERE location='Massachusetts' AND dept='sales';
SELECT * FROM users WHERE is_admin = 1;
SELECT * FROM users WHERE is_admin > 0;
```

## Delete Row

```sql
DELETE FROM users WHERE id = 2;
```

## Update Row

```sql
UPDATE users SET email = 'Thabo1@gmail.com' WHERE id = 2;

```

## Add New Column

```sql
ALTER TABLE users ADD age VARCHAR(3);
```

## Modify Column

```sql
ALTER TABLE users MODIFY COLUMN age INT(3);
```

## Order By (Sort)

```sql
SELECT * FROM users ORDER BY last_name ASC;
SELECT * FROM users ORDER BY last_name DESC;
```

## Concatenate Columns

```sql
SELECT CONCAT(first_name, ' ', last_name) AS 'Name', dept FROM users;

```

## Select Distinct Rows

```sql
SELECT DISTINCT location FROM users;

```

## Between (Select Range)

```sql
SELECT * FROM users WHERE age BETWEEN 20 AND 25;
```

## Like (Searching)

```sql
SELECT * FROM users WHERE dept LIKE 'd%';
SELECT * FROM users WHERE dept LIKE 'dev%';
SELECT * FROM users WHERE dept LIKE '%t';
SELECT * FROM users WHERE dept LIKE '%e%';
```

## Not Like

```sql
SELECT * FROM users WHERE dept NOT LIKE 'd%';
```

## IN

```sql
SELECT * FROM users WHERE dept IN ('design', 'sales');
```

## Create & Remove Index

```sql
CREATE INDEX LIndex On users(location);
DROP INDEX LIndex ON users;
```

## New Table With Foreign Key (Posts)

```sql
CREATE TABLE posts(
id INT AUTO_INCREMENT,
   user_id INT,
   title VARCHAR(100),
   body TEXT,
   publish_date DATETIME DEFAULT CURRENT_TIMESTAMP,
   PRIMARY KEY(id),
   FOREIGN KEY (user_id) REFERENCES users(id)
);
```

## Add Data to Posts Table

```sql
INSERT INTO posts(user_id, title, body) VALUES (1, 'Post One', 'This is post one'),(3, 'Post Two', 'This is post two'),(1, 'Post Three', 'This is post three'),(2, 'Post Four', 'This is post four'),(5, 'Post Five', 'This is post five'),(4, 'Post Six', 'This is post six'),(2, 'Post Seven', 'This is post seven'),(1, 'Post Eight', 'This is post eight'),(3, 'Post Nine', 'This is post none'),(4, 'Post Ten', 'This is post ten');
```

## INNER JOIN

```sql
SELECT
  users.first_name,
  users.last_name,
  posts.title,
  posts.publish_date
FROM users
INNER JOIN posts
ON users.id = posts.user_id
ORDER BY posts.title;
```

## New Table With 2 Foriegn Keys

```sql
CREATE TABLE comments(
	id INT AUTO_INCREMENT,
    post_id INT,
    user_id INT,
    body TEXT,
    publish_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY(id),
    FOREIGN KEY(user_id) references users(id),
    FOREIGN KEY(post_id) references posts(id)
);
```

## Add Data to Comments Table

```sql
INSERT INTO comments(post_id, user_id, body) VALUES (1, 3, 'This is comment one'),(2, 1, 'This is comment two'),(5, 3, 'This is comment three'),(2, 4, 'This is comment four'),(1, 2, 'This is comment five'),(3, 1, 'This is comment six'),(3, 2, 'This is comment six'),(5, 4, 'This is comment seven'),(2, 3, 'This is comment seven');
```

## Left Join

```sql
SELECT
comments.body,
posts.title
FROM comments
LEFT JOIN posts ON posts.id = comments.post_id
ORDER BY posts.title;

```

## Join Multiple Tables

```sql
SELECT
comments.body,
posts.title,
users.first_name,
users.last_name
FROM comments
INNER JOIN posts on posts.id = comments.post_id
INNER JOIN users on users.id = comments.user_id
ORDER BY posts.title;

```

## Aggregate Functions

```sql
SELECT COUNT(id) FROM users;
SELECT MAX(age) FROM users;
SELECT MIN(age) FROM users;
SELECT SUM(age) FROM users;
SELECT UCASE(first_name), LCASE(last_name) FROM users;

```

## Group By

```sql
SELECT age, COUNT(age) FROM users GROUP BY age;
SELECT age, COUNT(age) FROM users WHERE age > 20 GROUP BY age;
SELECT age, COUNT(age) FROM users GROUP BY age HAVING count(age) >=2;

```

# Additional Exericise

Here's a model of what you now have loaded in the your database. 
<img src="http://i.imgur.com/BirbWW5.png" />

Answer by showing the query:

* Using `count`, get the number of cities in the USA
* Find out what the population and average life expectancy for people in Argentina (ARG) is
* Using `IS NOT NULL, ORDER BY, LIMIT`, what country has the highest life expectancy?
* Using `LEFT JOIN, ON`, what is the capital of Spain (ESP)?
* Using `LEFT JOIN, ON`, list all the languages spoken in the 'Southeast Asia' region
* Select 25 cities around the world that start with the letter 'F' in a single SQL query.
