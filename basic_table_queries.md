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
 
 
