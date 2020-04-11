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
