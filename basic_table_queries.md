### Creating table with Constraint

```
CREATE TABLE person
 (person_id SMALLINT UNSIGNED,
  fname VARCHAR(20),
  lname VARCHAR(20),
  eye_color ENUM('BR','BL','GR'),
  birth_date DATE,
  street VARCHAR(30),
  city VARCHAR(20),
  state VARCHAR(20),
  country VARCHAR(20),
  postal_code VARCHAR(20),
  CONSTRAINT pk_person PRIMARY KEY (person_id)
 );
 ```
 
 Alternatively MySQL allows a check constraint to be attached to a column definition, as in the following:

```
eye_color CHAR(2) CHECK (eye_color IN ('BR','BL','GR')),
```

To view table description, 
 ```
 mysql> desc person;
 ```
 
 ```
  CREATE TABLE favorite_food
    (person_id SMALLINT UNSIGNED,
    food VARCHAR(20),
    CONSTRAINT pk_favorite_food PRIMARY KEY (person_id, food),
    CONSTRAINT fk_fav_food_person_id FOREIGN KEY (person_id)
    REFERENCES person (person_id)
  );
 ```
 ```
 mysql> desc favorite_food;
 ```
 
 
 #### Alter Table
 
 ```
 ALTER TABLE person MODIFY person_id SMALLINT UNSIGNED AUTO_INCREMENT;
 ```
 
 #### Insert Statement
 
 ```
 INSERT INTO person (person_id, fname, lname, eye_color, birth_date) VALUES (null, 'William','Turner', 'BR', '1972-05-27');
 ```
 
 #### Select Statemnt
 
 ```
 select person_id, fname, lname, birth_date from person where person_id=1;
 ```
 
 #### Update Statement
 ```
 UPDATE person SET street = '1225 Tremont St.', city = 'Boston', state = 'MA', country = 'USA', postal_code = '02138' WHERE person_id = 1;
 ```
 
 #### Delete Statement
 ```
 DELETE FROM person WHERE person_id = 2;
 ```
 
 #### Order By
 ```
 select * from favorite_food where person_id=1 order by food;
 ```
 
 #### Getting XML evrsion of data
 
 ```
 SELECT * FROM favorite_foodFOR XML AUTO, ELEMENTS
 ```
 
 #### Formatters that might be needed when converting strings to datetimes in MySQL
 
 ```
 %a The short weekday name, such as Sun, Mon, ...
%b The short month name, such as Jan, Feb, ...
%c The numeric month (0..12)
%d The numeric day of the month (00..31)
%f The number of microseconds (000000..999999)
%H The hour of the day, in 24-hour format (00..23)
%h The hour of the day, in 12-hour format (01..12)
%i The minutes within the hour (00..59)
%j The day of year (001..366)
%M The full month name (January..December)
%m The numeric month
%p AM or PM
%s The number of seconds (00..59)
%W The full weekday name (Sunday..Saturday)
%w The numeric day of the week (0=Sunday..6=Saturday)
%Y The four-digit year
 ```
 
