# SQL Injection
## What is SQL Injection?
SQL Injection (SQLi) is a web vulnerability that occurs when user input is directly inserted into a SQL query without proper validation or sanitization.

### An attacker can manipulate database queries to:
- bypass authentication
- access sensitive information
- dump database contents
- modify or delete data

### SQL Injection is one of the most dangerous web vulnerabilities because it directly affects the backend database.
--
##How SQL Injection Works

### Applications often take user input and place it inside SQL queries.

Example vulnerable query:
```SQL
SELECT first_name, last_name FROM users WHERE user_id = '$id';
```
### If the application does not sanitize user input properly, attackers can inject their own SQL statements.
--
##Testing for SQL Injection

To test whether the application is vulnerable, enter:
```SQL
1'
```
If the application throws a database error, it usually means:

user input is reaching the SQL query directly
quotes are not sanitized properly
the parameter may be vulnerable to SQL Injection

Image Placeholder: [Insert Image: sql_error_test.png]

The error reveals SQL syntax problems caused by the injected quote.

This confirms that the application is interacting unsafely with the database.



