# SQL Injection – DVWA

## What is SQL Injection?

SQL Injection (SQLi) is a web vulnerability that occurs when user input is directly inserted into a SQL query without proper validation or sanitization.

An attacker can manipulate database queries to:

- Bypass authentication
- Access sensitive information
- Dump database contents
- Modify or delete data

SQL Injection is one of the most dangerous web vulnerabilities because it directly affects the backend database.

--

## How SQL Injection Works

Applications often take user input and place it inside SQL queries.

### Example Vulnerable Query

```sql
SELECT first_name, last_name FROM users WHERE user_id = '$id';
```

If the application does not sanitize user input properly, attackers can inject their own SQL statements.

--

## Testing for SQL Injection

To test whether the application is vulnerable, enter:

```sql
1'
```

If the application throws a database error, it usually means:

- User input is reaching the SQL query directly
- Quotes are not sanitized properly
- The parameter may be vulnerable to SQL Injection

### Image Placeholder

```text
[Insert Image: sql_error_test.png]
```

The error reveals SQL syntax problems caused by the injected quote.

This confirms that the application is interacting unsafely with the database.

--

# SQL Injection – Low Security

In DVWA Low Security mode, there is almost no input validation or filtering.

## Navigate To

```text
DVWA → SQL Injection
```

## Set Security Level

```text
DVWA Security Level → Low
```

--

## Basic SQL Injection Payload

Test the following payload:

```sql
1' OR '1'='1
```

### Explanation

- `'1'='1'` is always `TRUE`
- The database query returns all available rows
- Instead of showing one user, the application dumps multiple users

### Image Placeholder

```text
[Insert Image: auth_bypass.png]
```

This demonstrates how attackers can manipulate SQL query logic.

--

## UNION-Based SQL Injection

UNION-based SQL Injection allows attackers to combine another SQL query with the original query.

### Payload

```sql
1' UNION SELECT 1,2-- -
```

### Explanation

- `UNION SELECT` appends another query result
- `1,2` is used to identify the number of columns
- `-- -` comments out the remaining original query

If the number of columns matches correctly, the application displays injected data.

--

## Database Enumeration

After confirming UNION injection works, attackers can enumerate database information.

### Payload

```sql
1' UNION SELECT database(),user()-- -
```

### Explanation

- `database()` returns the current database name
- `user()` returns the current database user

### Image Placeholder

```text
[Insert Image: database_enum.png]
```

### Observed

- Database name was disclosed
- Database user information was exposed

This confirms successful interaction with the backend database.

--

## Table Enumeration

Attackers can enumerate tables using `information_schema`.

### Payload

```sql
1' UNION SELECT table_name,2 FROM information_schema.tables-- -
```

### Explanation

- `information_schema.tables` stores metadata about database tables
- `table_name` extracts available table names

### Image Placeholder

```text
[Insert Image: table_enum.png]
```

The application successfully displayed multiple table names from the database.

This demonstrates how attackers can enumerate internal database structures.
