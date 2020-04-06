#### Query Mechanics

- Each connection to the MySQL server is assigned an identifier, which is shown to you when you first log in:

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.15 MySQL Community Server - GPL
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or itsaffiliates. Other names may be trademarks of their respectiveowners.
Type 'help;' or '\h' for help. Type '\c' to clear the buffer.
```

- Once the server has verified your username and password and issued you a connection, you are ready to execute queries (along with other SQL statements). Each time a query is sent to the server, the server checks the following things prior to statement execution:
  - Do you have permission to execute the statement?
  - Do you have permission to access the desired data?
  - Is your statement syntax correct?
 
- If your statement passes these three tests, then your query is handed to the query optimizer, whose job it is to determine the most efficient way to execute your query. The optimizer looks at such things as the order in which to join the tables named in your from clause and what indexes are available, and then it picks an execution plan, which the server uses to execute your query.

#### Query Clauses

Several components or clauses make up the select statement. While only one of them is mandatory when using MySQL (the select clause), you will usually include at least two or three of the six available clauses. Below table shows the different clauses and their purposes,

| Clause name | Purpose |
| -- | -- |
| select | Determines which columns to include in the query’s result set |
| from | Identifies the tables from which to retrieve data and how the tables should be joined |
| where | Filters out unwanted data |
| group by | Used to group rows together by common column values |
| having |Filters out unwanted groups |
| order by | Sorts the rows of the final result set by one or more columns |


#### The select Clause

- The select clause determines which of all possible columns should be included in the query’s result set.
- The below query demonstrates the use of a table column, a literal, an expression, and a built-in function call in a single query against the employee table,
  ```
  SELECT language_id, 'COMMON' language_usage, language_id * 3.1415927 lang_pi_value, upper(name) language_name FROM language;
  ```
- If you only need to execute a built-in function or evaluate a simple expression, you can skip the from clause entirely. Here’s an example:
  ```
  SELECT version(), user(), database();
  ```
  
#### Column Aliases

- There are two ways to name a returned column to be more convinient,
  - In below query language_usage, lang_pi_value and language_name are alias for column name,
  ```
  SELECT language_id, 'COMMON' language_usage, language_id * 3.1415927 lang_pi_value, upper(name) language_name FROM language;
  ```
  - Second way is to refre by **AS**,
  ```
  SELECT language_id, 'COMMON' AS language_usage, language_id * 3.1415927 AS lang_pi_value, upper(name) AS language_name FROM language;
  ```

#### Removing Duplicates (DISTINCT)
- In some cases, a query might return duplicate rows of data. In order so solve it use **DISTINCT**,
  ```
  SELECT DISTINCT actor_id FROM film_actor ORDER BY actor_id;
  ```
- **Warning**
  Keep in mind that generating a distinct set of results requires the data to be sorted, which can be time consuming for large result sets. Don’t fall into the trap of using distinct just to be sure there are no duplicates; instead, take the time to understand the data you are working with so that you will know whether duplicates are possible.


#### The from Clause

The from clause defines the tables used by a query, along with the means of linking the tables together.


### Tables

When confronted with the term table, most people think of a set of related rows stored in a database. While this does describe one type of table, I would like to use the word in a more general way by removing any notion of how the data might be stored and concentrating on just the set of related rows. Four different types of tables meet this relaxed definition:
- Permanent tables (i.e., created using the create table statement)
- Derived tables (i.e., rows returned by a subquery and held in memory)
- Temporary tables (i.e., volatile data held in memory)			
- Virtual tables (i.e., created using the create view statement)

#### Derived (subquery-generated) tables

- A subquery is a query contained within another query. Subqueries are surrounded by parentheses and can be found in various parts of a select statement; within the from clause, however, a subquery serves the role of generating a derived table that is visible from all other query clauses and can interact with other tables named in the from clause. Here’s a simple example:
 ```
  SELECT concat(cust.last_name, ', ', cust.first_name) full_name FROM (SELECT first_name, last_name, email FROM customer WHERE first_name = 'JESSIE') cust;
 ```
- In this example, a subquery against the customer table returns three columns, and the containing query references two of the three available columns. The subquery is referenced by the containing query via its alias, which, in this case, is cust. The data in cust is held in memory for the duration of the query and is then discarded. 


#### Temporary tables

- Although the implementations differ, every relational database allows the ability to define volatile, or temporary, tables. These tables look just like permanent tables, but any data inserted into a temporary table will disappear at some point (generally at the end of a transaction or when your database session is closed). Here’s a simple example showing how actors whose last names start with J can be stored temporarily:
  ```
  mysql> CREATE TEMPORARY TABLE actors_j (actor_id smallint(5), first_name varchar(45), last_name varchar(45));
  Query OK, 0 rows affected (0.00 sec)

  mysql> INSERT INTO actors_j SELECT actor_id, first_name, last_name FROM actor WHERE last_name LIKE 'J%';
  Query OK, 7 rows affected (0.03 sec)
  Records: 7  Duplicates: 0  Warnings: 0

  mysql> SELECT * FROM actors_j;
  // Results of query
  ```
- These result rows are held in memory temporarily and will disappear after your session is closed.


#### Views

- A view is a query that is stored in the data dictionary. It looks and acts like a table, but there is no data associated with a view (this is why I call it a virtual table). When you issue a query against a view, your query is merged with the view definition to create a final query to be executed.
- To demonstrate, here’s a view definition that queries the employee table and includes four of the available columns:
  ```
  CREATE VIEW cust_vw AS SELECT customer_id, first_name, last_name, active FROM customer;
  ```
- When the view is created, no additional data is generated or stored: the server simply tucks away the select statement for future use. Now that the view exists, you can issue queries against it, as in:
```
SELECT first_name, last_name FROM cust_vw WHERE active = 0;
```
- Views are created for various reasons, including to hide columns from users and to simplify complex database designs.


#### Table Links

- The second deviation from the simple from clause definition is the mandate that if more than one table appears in the from clause, the conditions used to link the tables must be included as well. This is not a requirement of MySQL or any other database server, but it is the ANSI-approved method of joining multiple tables, and it is the most portable across the various database servers.

  ```
  SELECT customer.first_name, customer.last_name, time(rental.rental_date) rental_time FROM customer INNER JOIN rental ON customer.customer_id = rental.customer_id WHERE date(rental.rental_date) = '2005-06-14';
  ```

#### Defining Table Aliases

- When multiple tables are joined in a single query, you need a way to identify which table you are referring to when you reference columns in the select, where, group by, having, and order by clauses. You have two choices when referencing a table outside the from clause:
  - Use the entire table name, such as employee.emp_id.
  - Assign each table an alias and use the alias throughout the query.

```
SELECT c.first_name, c.last_name,  time(r.rental_date) rental_time FROM customer c  INNER JOIN rental r  ON c.customer_id = r.customer_id WHERE date(r.rental_date) = '2005-06-14';
```
 
```
SELECT c.first_name, c.last_name,  time(r.rental_date) rental_time FROM customer AS c  INNER JOIN rental AS r  ON c.customer_id = r.customer_id WHERE date(r.rental_date) = '2005-06-14';
```

#### The order by Clause

- In general, the rows in a result set returned from a query are not in any particular order. If you want your result set to be sorted, you will need to instruct the server to sort the results using the order by clause:

```
The order by clause is the mechanism for sorting your result set using either raw column data or expressions based on column data.
```

```
SELECT c.first_name, c.last_name, time(r.rental_date) rental_time FROM customer c INNER JOIN rental r ON c.customer_id = r.customer_id WHERE date(r.rental_date) = '2005-06-14' ORDER BY c.last_name, c.first_name;
```

#### Ascending Versus Descending Sort Order
- When sorting, you have the option of specifying ascending or descending order via the asc and desc keywords. The default is ascending, so you will need to add the desc keyword if you want to use a descending sort. 

```
SELECT c.first_name, c.last_name, time(r.rental_date)  FROM customer c INNER JOIN rental r ON c.customer_id = r.customer_id WHERE date(r.rental_date) = '2005-06-14' ORDER BY time(r.rental_date) desc;
```

- Descending sorts are commonly used for ranking queries, such as “show me the top five account balances.” MySQL includes a limit clause that allows you to sort your data and then discard all but the first X rows.

#### Sorting via Numeric Placeholders

- If you are sorting using the columns in your select clause, you can opt to reference the columns by their position in the select clause rather than by name. This can be especially helpful if you are sorting on an expression

```
SELECT c.first_name, c.last_name, time(r.rental_date) rental_time FROM customer c INNER JOIN rental r ON c.customer_id = r.customer_id WHERE date(r.rental_date) = '2005-06-14' ORDER BY 3 desc;
```
- Here ORDER BY 3 means ORDER BY time(r.rental_date)


