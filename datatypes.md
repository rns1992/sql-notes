## SQL Datatypes

### Character Data

Character data can be stored as either fixed-length or variable-length strings; the difference is that fixed-length strings are right-padded with spaces and always consume the same number of bytes, and variable-length strings are not right-padded with spaces and don’t always consume the same number of bytes. When defining a character column, you must specify the maximum size of any string to be stored in the column. For example, if you want to store strings up to 20 characters in length, you could use either of the following definitions:
- char(20)    /* fixed-length */
- varchar(20) /* variable-length */

The maximum length for char columns is currently 255 bytes, whereas varchar columns can be up to 65,535 bytes. 

An exception is made in the use of varchar for Oracle Database. Oracle users should use the varchar2 type when defining variable-length character columns.

### Text data

If you need to store data that might exceed the 64 KB limit for varchar columns, you will need to use one of the text types.

| Text type	|	Maximum number of bytes |
|-----------|-------------------------|
| tinytext | 255 |
| text | 65,535 |
| mediumtext | 16,777,215 |
| longtext | 4,294,967,295 |

When choosing to use one of the text types, you should be aware of the following:

 - If the data being loaded into a text column exceeds the maximum size for that type, the data will be truncated.
 - Trailing spaces will not be removed when data is loaded into the column.
 - When using text columns for sorting or grouping, only the first 1,024 bytes are used, although this limit may be increased if necessary.
 - The different text types are unique to MySQL. SQL Server has a single text type for large character data, whereas DB2 and Oracle use a data type called clob, for Character Large Object.
 
#### NOTE

Oracle Database allows up to 2,000 bytes for char columns and 4,000 bytes for varchar2 columns. For larger documents you may use the clob type. SQL Server can handle up to 8,000 bytes for both char and varchar data, but you can store up to 2 GB of data in a column defined as varchar(max).
 - Now that MySQL allows up to 65,535 bytes for varchar columns (it was limited to 255 bytes in version 4), there isn’t any particular need to use the tinytext or text type.
 
 ### Numeric Data
 
 To handle these types of data (and more), MySQL has several different numeric data types. The most commonly used numeric types are those used to store whole numbers, or integers. When specifying one of these types, you may also specify that the data is unsigned, which tells the server that all data stored in the column will be greater than or equal to zero.
 
| Type | Signed range | Unsigned range |
|------|--------------|----------------|
| tinyint | −128 to 127 | 0 to 255 |
| smallint | −32,768 to 32,767 | 0 to 65,535 |
| mediumint | −8,388,608 to 8,388,607 | 0 to 16,777,215 |
| int | −2,147,483,648 to 2,147,483,647 | 0 to 4,294,967,295 |
| bigint | −2^63 to 2^63 - 1 | 0 to 2^64 - 1 |


For floating-point numbers (such as 3.1415927), you may choose from the numeric types shown below,

| Type | Numeric range |
| --------------- | ---------- |
| float(p , s) | −3.402823466E+38 to −1.175494351E-38 and 1.175494351E-38 to 3.402823466E+38 |
| double(p , s) | −1.7976931348623157E+308 to −2.2250738585072014E-308			and 2.2250738585072014E-308 to 1.7976931348623157E+308 |

For example, a column defined as float(4,2) will store a total of four digits, two to the left of the decimal and two to the right of the decimal. Therefore, such a column would handle the numbers 27.44 and 8.19 just fine, but the number 17.8675 would be rounded to 17.87, and attempting to store the number 178.375 in your float(4,2) column would generate an error.

### Temporal Data

MySQL includes data types to handle all of these situations. Table shows the temporal data types supported by MySQL.

| Type | Default format | Allowable values |
| -- | --- | -- |
| date | YYYY-MM-DD | 1000-01-01 to 9999-12-31 |
| datetime | YYYY-MM-DD HH:MI:SS | 1000-01-01 00:00:00.000000 to 9999-12-31 23:59:59.999999 |
| timestamp | YYYY-MM-DD HH:MI:SS | 1970-01-01 00:00:00.000000 to 2038-01-18 22:14:07.999999 |
| year | YYYY | 1901 to 2155 | 
| time | HHH:MI:SS | −838:59:59.000000 to 838:59:59.000000 |


