### Condition Evaluation

A where clause may contain one or more conditions, separated by the operators and and or. If multiple conditions are separated only by the and operator, then all the conditions must evaluate to true for the row to be included in the result set. 

### Using the not Operator

```
WHERE NOT (first_name = 'STEVEN' OR last_name = 'YOUNG')  AND create_date > '2006-01-01'
```

While it is easy for the database server to handle, it is typically difficult for a person to evaluate a where clause that includes the not operator, which is why you won’t encounter it very often. In this case, you can rewrite the where clause to avoid using the not operator:

```
WHERE first_name <> 'STEVEN' AND last_name <> 'YOUNG'  AND create_date > '2006-01-01'
```

### Building a Condition

A condition is made up of one or more expressions combined with one or more operators. An expression can be any of the following:

- A number
- A column in a table or view
- A string literal, such as 'Maple Street'			
- A built-in function, such as concat('Learning', ' ', 'SQL')
- A subquery
- A list of expressions, such as ('Boston', 'New York', 'Chicago')

The operators used within conditions include:

- Comparison operators, such as =, !=, <, >, <>, like, in, and between
- Arithmetic operators, such as +, −, *, and /


### Condition Types

#### Equality Conditions

A large percentage of the filter conditions that you write or come across will be of the form 'column = expression'

#### Inequality conditions

Another fairly common type of condition is the inequality condition, which asserts that two expressions are not equal. 

```
SELECT c.email FROM customer c INNER JOIN rental r ON c.customer_id = r.customer_id WHERE date(r.rental_date) <> '2005-06-14';
```

#### Range Conditions

Along with checking that an expression is equal to (or not equal to) another expression, you can build conditions that check whether an expression falls within a certain range. This type of condition is common when working with numeric or temporal(Date) data. 

```
SELECT customer_id, rental_date FROM rental WHERE rental_date < '2005-05-25';
```

#### THE BETWEEN OPERATOR

When you have both an upper and lower limit for your range, you may choose to use a single condition that utilizes the between operator rather than using two separate conditions.

```
SELECT customer_id, rental_date FROM rental WHERE rental_date BETWEEN '2005-06-14' AND '2005-06-16';
```

When using the between operator, there are a couple of things to keep in mind. You should always specify the lower limit of the range first (after between) and the upper limit of the range second (after and). 

#### String ranges

While ranges of dates and numbers are easy to understand, you can also build conditions that search for ranges of strings, which are a bit harder to visualize. Say, for example, you are searching for customers whose last name falls within a range. Here’s a query that returns customers whose last name falls between FA and FR:

```
SELECT last_name, first_name FROM customer WHERE last_name BETWEEN 'FA' AND 'FR';
```

+------------+------------+
| last_name  | first_name |
+------------+------------+
| FARNSWORTH | JOHN       |
| FENNELL    | ALEXANDER  |
| FERGUSON   | BERTHA     |
| FERNANDEZ  | MELINDA    |
| FIELDS     | VICKI      |
| FISHER     | CINDY      |
| FLEMING    | MYRTLE     |
| FLETCHER   | MAE        |
| FLORES     | JULIA      |
| FORD       | CRYSTAL    |
| FORMAN     | MICHEAL    |
| FORSYTHE   | ENRIQUE    |
| FORTIER    | RAUL       |
| FORTNER    | HOWARD     |
| FOSTER     | PHYLLIS    |
| FOUST      | JACK       |
| FOWLER     | JO         |
| FOX        | HOLLY      |
+------------+------------+

While there are five customers whose last name starts with FR, they are not included in the results, since a name like FRANKLIN is outside of the range. However, we can pick up four of the five customers by extending the righthand range to FRB:

```
SELECT last_name, first_name FROM customer WHERE last_name BETWEEN 'FA' AND 'FRB';
```

#### Membership Conditions

```
SELECT title, rating FROM film WHERE rating = 'G' OR rating = 'PG';
```

While this where clause (two conditions or’d together) wasn’t too tedious to generate, imagine if the set of expressions contained 10 or 20 members. For these situations, you can use the in operator instead:

```
SELECT title, ratingFROM filmWHERE rating IN ('G','PG');
```

#### Using subqueries

Along with writing your own set of expressions, such as ('G','PG'), you can use a subquery to generate a set for you on the fly. For example, if you can assume that any film whose title includes the string 'PET' would be safe for family viewing, you could execute a subquery against the film table to retrieve all ratings associated with these films and then retrieve all films having any of these ratings:

```
SELECT title, rating FROM film WHERE rating IN (SELECT rating FROM film WHERE title LIKE '%PET%');
```


#### Using not in

Sometimes you want to see whether a particular expression exists within a set of expressions, and sometimes you want to see whether the expression does not exist within the set. For these situations, you can use the not in operator.

```
SELECT title, ratingFROM filmWHERE rating NOT IN ('PG-13','R', 'NC-17');
```


#### Matching Conditions

So far, you have been introduced to conditions that identify an exact string, a range of strings, or a set of strings; the final condition type deals with partial string matches. You may, for example, want to find all customers whose last name begins with Q. You could use a built-in function to strip off the first letter of the last_name column, as in the following:

```
SELECT last_name, first_name FROM customer WHERE left(last_name, 1) = 'Q';
```

#### Using wildcards

When searching for partial string matches, you might be interested in:

- Strings beginning/ending with a certain character			
- Strings beginning/ending with a substring			
- Strings containing a certain character anywhere within the string			
- Strings containing a substring anywhere within the string			
- Strings with a specific format, regardless of individual characters

| Wildcard character | 	Matches |
| -- | -- |
| _ | Exactly one character |
| % |  Any number of characters (including 0) |
